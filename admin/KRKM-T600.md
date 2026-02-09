# 🧪 Тест-кейс: Код 200. Администратор. Смена статуса сотрудника c "Активный" на "Уволен" /api/admin/employees/status/{employeeId}.

**ID:** KRKM-T600  
**Приоритет:** 🔴 Высокий  
**Статус:** ✅ Активный  
**Автор:** Лапотышкин Сергей  
**Дата создания:** 2025-11-11  
**Тип тестирования:** Функциональное


---

## 🎪 Предусловия
- Пройдена авторизация, получен токен (Admin)
- Сотрудник есть в бд ЛК сотрудника и имеет статус "Активный"
- Открыта бд ЛК сотрудника



---

## 📝 Шаги выполнения

| Действие | Тестовые данные  |Ожидаемый результат | 
|----------| ----- |-------------------|
| 1. Выполнить sql-запрос | SELECT T_EMPLOYEE_INFO.ID , EMPLOYEE_STATUS_NAME, ACCESS_code  <br>FROM T_EMPLOYEE_INFO <br> INNER JOIN DICT_EMPLOYEE_STATUS ON T_EMPLOYEE_INFO.STATUS = DICT_EMPLOYEE_STATUS.ID <br> INNER JOIN DICT_ACCESS on T_EMPLOYEE_INFO.HAS_ACCESS = DICT_ACCESS.ID LIMIT 1;  |  employee_status_name = "Активный" access_code = 'Exist'  |
| 2. Отправить PATCH запрос на end-point admin/employees/status/{{employeeId из шага 1}} |{"employeeStatus": "Уволен"} |Запрос успешно отправлен |
| 3. Перейти во вкладку logs в Kubernetes UI | тут был url Kubernetes |В логах есть сообщения <br> <<< [2025-12-16T13:01:03.067Z] [INFO ] [manager-account-app] [6941580f4d1411976f586a5ad1b878e3,7aee20bc09832cf0] [http-nio-8080-exec-9] [r.i.lab.controller.AdminController] : Получен запрос на изменение статуса сотрудника >>><br><<< [2025-12-16T13:01:03.071Z] [INFO ] [manager-account-app] [6941580f4d1411976f586a5ad1b878e3,7aee20bc09832cf0] [http-nio-8080-exec-9] [r.i.lab.controller.AdminController] : Запрос на изменение статуса сотрудника успешно обработан >>> |
| 4. Проверить ответ API|   | HTTP: 200 OK <br>{<br>"code": "SUCCESS",<br>"message": "Запрос успешно обработан",<br>"dateTime": "2025-12-16T13:01:03.069686Z",<br> "showData": true<br>}
| 5. Выполнить sql-запрос | SELECT T_EMPLOYEE_INFO.ID , EMPLOYEE_STATUS_NAME, ACCESS_code FROM T_EMPLOYEE_INFO<br>INNER JOIN DICT_EMPLOYEE_STATUS ON T_EMPLOYEE_INFO.STATUS = DICT_EMPLOYEE_STATUS.ID<br>INNER JOIN DICT_ACCESS on T_EMPLOYEE_INFO.HAS_ACCESS = DICT_ACCESS.ID<br>WHERE T_EMPLOYEE_INFO.ID = 'd3333333-5c7d-4e15-bb32-7f6a1c9a4d21'| Поле access_code ="not_exist" при статусе "уволен" |


