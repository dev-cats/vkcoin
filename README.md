# vkcoin
Враппер для платёжного API VK Coin. https://vk.com/@hs-marchant-api
# Установка
* Скачайте и установите [Python](https://www.python.org/downloads/) версии 3.6 и выше
* Введите следующую команду в командную строку:
```bash
pip install vkcoin
```
* Или, вы можете собрать библиотеку с GitHub:
```bash
pip install git+git://github.com/crinny/vkcoin.git
```
* Вы прекрасны!
# Начало работы
Для начала разработки, необходимо создать в своей папке исполняемый файл с расширением **.py**, например **test.py**. Вы не можете назвать файл vkcoin.py, так как это приведёт к конфликту. Теперь файл нужно открыть и импортировать библиотеку:
```python
import vkcoin

merchant = vkcoin.Merchant(user_id=123456789, key='xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
```
|Параметр|Тип|Описание|
|-|-|-|
|user_id|Integer|ID аккаунта ВКонтакте|
|key|String|Ключ для взаимодействия с API|
# Методы
Необязательные параметры при вызове функций выделены _курсивом_.

[`get_payment_url`](https://vk.com/@hs-marchant-api?anchor=ssylka-na-oplatu) - получет ссылку на оплату VK Coin
```python
result = merchant.get_payment_url(amount=10, payload=78922, free_amount=False)
print(result)
```
|Параметр|Тип|Описание|
|-|-|-|
|amount|Float|Количество VK Coin для перевода|
|_payload_|Integer|Число от -2000000000 до 2000000000, вернется в списке транзаций|
|_free_amount_|Boolean|True, что бы позволить пользователю изменять сумму перевода|
#
[`get_transactions`](https://vk.com/@hs-marchant-api?anchor=poluchenie-spiska-tranzaktsy) - получает список ваших транзакций
```python
result = merchant.get_transactions(tx=[1])
print(result)
```
|Параметр|Тип|Описание|
|-|-|-|
|tx|List|Массив ID переводов для получения или [1] - последние 1000 транзакций, [2] - 100|
|_last_tx_|Integer|Если указать номер последней транзакции то будут возвращены только транзакции после указанной|
#
[`send`](https://vk.com/@hs-marchant-api?anchor=perevod) - делает перевод другому пользователю
```python
result = merchant.send(amount=100, to_id=371576679)  # Если запустить этот код, вы переведёте мне 100 VK Coin :)
print(result)
```
|Параметр|Тип|Описание|
|-|-|-|
|amount|Float|Сумма перевода|
|to_id|Integer|ID аккаунта, на который будет совершён перевод|
#
[`get_balance`](https://vk.com/@hs-marchant-api?anchor=poluchenie-balansa) - возвращает баланс аккаунта
```python
result = merchant.get_balance(123456789, 987654321)
print(result)
```
|Тип|Описание|
|-|-|-|
Integer|ID аккаунтов, баланс которых нужно получить|
#
`register_payment_callback` - регистрирует callback ункцию, которая будет вызвана прим получении перевода
```python
merchant.register_payment_callback(token='xxxxxxxxxxxxxxxxxxxxxxxxxxxxx', on_payment=on_payment_recieved)
```
|Параметр|Тип|Описание|
|-|-|-|
|token|String|Токен VK API, полученный по [инструкции](https://github.com/crinny/vkcoin#получение-токена) (необходим только для _on_payment_)|
|on_payment|Function|Callback функция, которая будет вызвана при получении перевода от других пользователей|
# Callback
Описание параметров, которые будут переданы в callback функции.

`on_payment` - вызывается при получении перевода от других пользователей
```python
def on_payment_recieved(user_id, amount):
    print(user_id, amount)
```
|Параметр|Тип|Описание|
|-|-|-|
|user_id|Integer|ID аккаунта, с которого был совершён перевод|
|amount|Float|Сумма перевода|
# Примеры
### Callback
```python
import vkcoin

def on_payment_recieved(user_id, amount):
    print(user_id, amount)

merchant = vkcoin.Merchant(user_id=123456789, key='xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
merchant.register_payment_callback(token='xxxxxxxxxxxxxxxxxxxxxxxxxxxxx', callback=on_payment_recieved)

```
# Получение токена
Перейдите по [ссылке](https://vk.cc/9f4IXA), нажмите "Разрешить" и скопируйте часть адресной строки после access_token= и до &expires_in (85 символов)

# Где меня можно найти
Могу ответить на ваши вопросы
* [ВКонтакте](https://vk.com/crinny)
* [Telegram](https://t.me/truecrinny)
* [Чат ВКонтакте по VK Coin API](https://vk.me/join/AJQ1d5eSUQ81wnwgfHSRktCi)