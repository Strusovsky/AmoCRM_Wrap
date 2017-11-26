# Обёртка для простого взаимодействия с Api AmoCRM

## Подключение

В самом начале файла в котором вы планируете использовать обёртку. Возможно убрать лишние не используемые классы.
```php
<?php
use AmoCRM\Amo;
use AmoCRM\Contact;
use AmoCRM\Lead;
```
Далее, в любом месте перед использованием обёртки, нужно подключить файл автозагрузки классов 'autoload.php'. Путь к файлу конечно же может отличаться от примера ниже.
```php
<?php require_once 'AmoCRM_Wrap/autoload.php'; ?>
```
## Использование
Для начала нужно произвести авторизацию. Для этого вызываем статический метод 'authorization' у класса 'Амо' в который необходимо передать:

1. Субдомен црм
2. Логин юзера(почта)
3. Хэш этого юзера

```php
<?php
use AmoCRM\Amo;

$amo = new Amo('test', 'test@test.ru', '011c2d7f862c688286b43ef552fb17f4');
if ($amo->isAuthorization()) {
    echo 'Всё норм';
} else {
    echo 'что-то пошло не так';
}
?>
```

## Возможности
* Статичный метод для очистки телефона от лишних символов. Применяется при поиске по контактам.
* Поиск контакта по телефону или почте. Возвращается массив из "Контактов" которые имеют или данный телефон или почту). Даный метод можно использовать для поиска дублей.
* Создание, изменение и удаление контактов, сделок, компаний, заметок и задач
* Прикрепление к контактам, сделкам и компаниям файлов
* Удобная работа с доп полями, без знания их id и т.п.
* На все запросы возвращается не огромный масив с данными, а удобные для работы объекты (Contact, Lead и тп)

## В ближайших плана
* Добавлени возможности работы с новыми сущностями (Покупатели и тп)

## Благодарности
* Владислав Алаторцев — За уникальные методы прикрепления файлов и удаления сущностей

# Описание методов основных классов

## Класс Amo
* **Amo::clearPhone($phone)** — Статичный метод отчистки телефона от лишних символов. Возвращяет (int)
* **__construct($domain, $userLogin, $userHash)** — Метод котрый вызывается при создании главного объекта класса Амо. Пытается произвести авторизацию.
* **isAuthorization()** — Проверяет текущую авторизацию и возвращает true в случаее успешной авторизации и false в случае провала
* **searchContact($phone, $email)** — Производит поиск по контактам. При этом не учитывает формат телефона (телефоны начинающиеся с 7 или 8 считает разными). Возвращяет нумерованый массив из объектов класса Contact, где ключём будет являть id контакта
* **searchLead($query)** — Производит поиск по сделкам по произвольной строке. Возвращяет нумерованый массив из объектов класса Lead, где ключём будет являть id сделки
* **contactsList($query = null, $limit = 500, $offset = 0, $responsibleUsersIdOrName = array(), \DateTime $modifiedSince = null)** — Производит поиск по контактам и возвращает массив объектов класса Contact. Параметры: $query - произвольная строка, $limit - лимит количества объектов в результате (техническое ограничение не более 500), $offset - отступ от начала списка, $responsibleUsersIdOrName - один элемент или массив ответственных (принимает как id так и имена или часть имени), $modifiedSince - дата и время после которых контакт был изменён
* **leadsList($query = null, $limit = 500, $offset = 0, $responsibleUsersIdOrName = array(), \DateTime $modifiedSince = null)** — Производит поиск по сделкам и возвращает массив объектов класса Lead. Параметры: $query - произвольная строка, $limit - лимит количества объектов в результате (техническое ограничение не более 500), $offset - отступ от начала списка, $responsibleUsersIdOrName - один элемент или массив ответственных (принимает как id так и имена или часть имени), $modifiedSince - дата и время после которых сделка была изменена
* **companyList($query = null, $limit = 500, $offset = 0, $responsibleUsersIdOrName = array(), \DateTime $modifiedSince = null)** — Производит поиск по компаниям и возвращает массив объектов класса Company. Параметры: $query - произвольная строка, $limit - лимит количества объектов в результате (техническое ограничение не более 500), $offset - отступ от начала списка, $responsibleUsersIdOrName - один элемент или массив ответственных (принимает как id так и имена или часть имени), $modifiedSince - дата и время после которых компания была изменена
* **tasksList($query = null, $limit = 500, $offset = 0, $responsibleUsersIdOrName = array(), \DateTime $modifiedSince = null)** — Производит поиск по задачам и возвращает массив объектов класса Task. Параметры: $query - произвольная строка, $limit - лимит количества объектов в результате (техническое ограничение не более 500), $offset - отступ от начала списка, $responsibleUsersIdOrName - один элемент или массив ответственных (принимает как id так и имена или часть имени), $modifiedSince - дата и время после которых задача была изменена
* **notesContactList($query = null, $limit = 500, $offset = 0, $responsibleUsersIdOrName = array(), \DateTime $modifiedSince = null)** — Производит поиск по заметкам у контатов и возвращает массив объектов класса Note. Параметры: $query - произвольная строка, $limit - лимит количества объектов в результате (техническое ограничение не более 500), $offset - отступ от начала списка, $responsibleUsersIdOrName - один элемент или массив ответственных (принимает как id так и имена или часть имени), $modifiedSince - дата и время после которых задача была изменена
* **notesLeadList($query = null, $limit = 500, $offset = 0, $responsibleUsersIdOrName = array(), \DateTime $modifiedSince = null)** — Производит поиск по заметкам у сделок и возвращает массив объектов класса Note. Параметры: $query - произвольная строка, $limit - лимит количества объектов в результате (техническое ограничение не более 500), $offset - отступ от начала списка, $responsibleUsersIdOrName - один элемент или массив ответственных (принимает как id так и имена или часть имени), $modifiedSince - дата и время после которых задача была изменена
* **notesCompanyList($query = null, $limit = 500, $offset = 0, $responsibleUsersIdOrName = array(), \DateTime $modifiedSince = null)** — Производит поиск по заметкам у компаний и возвращает массив объектов класса Note. Параметры: $query - произвольная строка, $limit - лимит количества объектов в результате (техническое ограничение не более 500), $offset - отступ от начала списка, $responsibleUsersIdOrName - один элемент или массив ответственных (принимает как id так и имена или часть имени), $modifiedSince - дата и время после которых задача была изменена
* **notesTaskList($query = null, $limit = 500, $offset = 0, $responsibleUsersIdOrName = array(), \DateTime $modifiedSince = null)** — Производит поиск по заметкам у задач(рехультат задачи) и возвращает массив объектов класса Note. Параметры: $query - произвольная строка, $limit - лимит количества объектов в результате (техническое ограничение не более 500), $offset - отступ от начала списка, $responsibleUsersIdOrName - один элемент или массив ответственных (принимает как id так и имена или часть имени), $modifiedSince - дата и время после которых задача была изменена

## Абстрактный класс Base

На этом классе базируются все другие основные классы. Все методы присутствующие в нём можно использовать в других классах.

* **__construct($id = null)** — Метод вызываемый при создании экземпляра класса. Можно передать id нужной сущности и тогда произойд\т загрузка данный из црм. Если не передавать id то будет создан новый объект. Если заполнить и сохранить его, он появится в црм.
* **save()** — Сохраняет текущий объект в црм. Возвращает true в случае успеха и false в случае ошибки.
* **delete()** — Удаляет текущий объект в црм. Возвращает true в случае успеха и false в случае ошибки.
* **getId()** — Возвращает id сущьности
* **getName()** — Возвращает наименование сущьности
* **setName($name)** — Изменяет имя на $name
* **getResponsibleUserId()** — Возвращает id ответственного за сущьность
* **getResponsibleUserName()** — Возвращает имя ответственного за сущьность
* **setResponsibleUser($responsibleUserIdOrName)** — Устанавливает ответственного за сущьность. Принимает либо id ответственного либо его имя (ищет эту строку во всех именах всех менеджеров црм). Возвращает true если такой менеджер есть или false если такого нет
* **getDateCreate()** — Возвращает объект DateTime с датой и временем создания сущности
* **getLastModified()** — Возвращает объект DateTime с датой и временем последнего изменения сущности
* **getModifiedUserId()** — Возвращает id человека который последний раз изменял сущность
* **getModifiedUserName()** — Возвращает имя человека который последний раз изменял сущность
* **getCreatedUserId()** — Возвращает id человека создавшего сущьность
* **getCreatedUserName()** — Возвращает имя человека создавшего сущьность
* **getLinkedCompanyId()** — Возвращает id компании к которой привязан контакт
* **setLinkedCompanyId($linkedCompanyId)** — Устанавливает id компании к которой будет привязан контакт
* **getTags()** — Возвращает ассоциативный массив тэгов id => name
* **addTag($tag)** — Добавляет тэг к имеющимся
* **delTag($tag)** — Ищет и удаляет тэг из имеющихся. Возвращает true или false если $tag не найден
* **getPhones()** — Нумерованый массив телефонов
* **addPhone($phone, $enum = 'OTHER')** — Добавляет телефон $phone к текущим, не обязательны пораметр тип телефона, по умолчанию "Другой". Возвращяет true или false в случае если тип телефона не корректный. Проверяет имеется ли уже такой телефон у контакт и не добавляет дубликат
Возможные варианты: WORK - Рабочий, WORKDD - Прямой, MOB - Мобильный, FAX - Факс, HOME - Домашний, OTHER - Другой
* **delPhone($phone)** — Ищет и удаляет телфон из имеющихся. Не учитывает формат телефона. Учитывает начинается телефон с 7 или 8. Возвращает true или false если $phone не найден
* **getEmails()** — Нумерованый массив email'ов
* **addEmails($email, $enum = 'OTHER')** — Добавляет почту $email к текущим, не обязательны пораметр тип почты, по умолчанию "Другой". Возвращяет true или false в случае если тип почты не корректный. Проверяет имеется ли уже такая почта у контакт и не добавляет дубликат. Возможные варианты: WORK - Рабочая, PRIV - Личная, OTHER - Другая
* **delEmail($email)** — Ищет и удаляет почту из имеющихся. Возвращает true или false если $email не найден
* **getLinkedLeadsId()** — Нумерованый массив id сделок привязанных к контакту
* **addLinkedLeadId($linkedLeadId)** — Добавляет id сделки к имеющимся привязанным к контакту
* **delLinkedLeadId($linkedLeadId)** — Ищет и удаляет id сделки из имеющихся. Возвращает true или false если $linkedLeadId не найден
* **addCustomField($name, $type)** — Создаёт кастомное поле у текущей сущьности с именем $name и типом $type которое может быть: 1 - Обыное текстовое поле, 2 - Текстовое поля с возможностью записывать только цифры, 3 - Поле обозначающее только наличие или отсутствие свойства (например: "да"/"нет"), 4 - Поле типа список с возможностью выбора одного элемента, 5 - Поле типа список c возможностью выбора нескольких элементов списка, 6 - Поле типа дата в формате (Год-Мес-День Час:Мин:Сек), 7 - Обычное текстовое поле предназначенное URL адресов, 8,9 - Поле содержащее большое количество текста, 10 - Поле типа переключатель, 11 - Поле короткой записи адреса, 12 - Поле адрес (в интерфейсе является набором из нескольких полей), 13 - Поле типа дата поиск по которому осуществляется без учета года т.е. день рождение. Формат (Год-Мес-День Час:Мин:Сек). Возвращает id поля в слуае успеха иначе false
* **delCustomField($nameOrId)** — Удаляет кастомное поле у текущей сущьности, принимает как id поля так и его имя. Возвращает true в случае успеха иначе false
* **getCustomField($nameOrId)** — Принимает имя кастомного поля или его id. Возвращает его значение в виде строки. Если значений несколько, то они перечесляются через точку с запятоу (;)
* **getCustomFields()** — Возвращает все значения кастомных полей в виде ассоциативного массива (имя поля => его значение). Если у поля несколько значений, то они перечесляются через точку с запятоу (;)
* **setCustomField($customFieldNameOrId, $value = null)** — Задаёт значение кастомного поля. Первым аргументов можно передать как id поля, так и его название в црм. Если не передавать значение ($value) или задать его пустым, то при сохранении поле в црм так же будет отчишено. В поле типа мультисписок значения вносятся с разделителем точка с запятоу (;) т.е. "первое; второе; третье"
* **addNote($text)** — Добавляет текстовую заметку для текущей сущности
* **addTask($text, $responsibleUserIdOrName = null, $completeTill, $typeId = 3)** — Добавляет задачу для текущей сущности.
Первый параметр - текст задачи.
Второй(не обязательный) - ответственный, принимает как id так и имя, если не установлен то ответственный тот же что и у текущей сущности.
Третий(не обязательный) - дата и время, в виде объекта класса DateTime, до которого нужно завершить задачу, если не указан то время устанавливается текущее.
Четвёртый(не обязательный) - тип задачи см. варианты в црм, принимает только имя типа, если не установлен принимает тип 3 - письмо.

## Классы Contact и Company

Все методы базовые


## Класс Lead

* **getPrice()** — Возвращает бюджет
* **setPrice($price)** — Задаёт бюджет
* **getPipelineId()** — Возвращает id воронки
* **getPipelineName()** — Возвращает название воронки
* **getStatusId()** — Возвращает id статуса
* **getStatusName()** — Возвращает название статуса
* **setStatus($idOrNamePipeline, $idOrNameStatus)** — Задаёт воронку и статус. Принимает id либо название из црм, как воронки так и статуса. Возвращает true в случае успеха, либо в случаее если хотя бы один из параметров не найден false
* **getMainContactId()** — Возвращает id основного контакта
* **setMainContactId($mainContactId)** — Задаёт id основного контакта
* **isClosed()** — Возвращает true если сделка закрыта и false в противном случае

## Класс Note и Task

* **getElementId()** — Возвращает id сущности к которому будет привязана заметка или задача
* **setElementId($elementId)** — Задаёт id сущности к которому будет привязана заметка или задача
* **getElementType()** — Возвращает тип сущности к которому будет привязана заметка или задача
Возможные значения: 1 - Контакт, 2 - Сделка, 3 - Компания, 4 - Результат задачи
* **setElementType($elementType)** — Устанавливает тип сущности к которому будет привязана заметка или задача. Воможные варианты таке же как выше
* **getType()** — Возвращает тип задачи или заметки. Варианты см. в црм
* **setType($type)** — Задаёт тип задачи или заметки. Варианты см. в црм
* **getText()** — Возвращает текст задачи или заметки
* **setText($text)** — Задаёт текст задачи или заметки

## Класс Unsorted

* **__construct($formName, $contacts, $lead, $pipelineIdOrName = null, $companies = array())** — Метод вызывающийся при создании объекта этого класса. $formName - название формы которое будет отображаться в интерфейсе црм. $contacts - массив объектов Contact которые будут созданы и привязаны к сделки после принятия заявки в "неразобранном". $lead - сделка котороя будет создана после принятия заявки в "неразобранном". $pipelineIdOrName - Воронка в которой будет создана заявка, необязательный параметр. $companies - необязательный параметр, массив объектов Сompanies которые будут созданы и привязаны к сделки после принятия заявки в "неразобранном"

## Классы хэлперы Info, CustomField, Value, Note и Task
Созданы как вспомогательные по этому описывать их не буду)

# Примеры

## Поиск, изменение и сохранение контакта в црм
```php
<?php
use AmoCRM\Amo;

$amo = new Amo('test', 'test@test.ru', '011c2d7f862c688286b43ef552fb17f4');
if ($amo->isAuthorization()) {
    $contacts =  $amo->searchContact('79998887766', 'test@test.ru'); //Ищем контакт по телефону и почте
    $contact = current($contacts); //берём первый найденый контакт
    $contact->setName($contact->getName() . ' лучший'); //Меняем имя дописывая в текущее строчку
    $contact->addPhone('78889998887766', 'MOB'); //Добавляем мобильный телефон
    $contact->addEmail('test2@test.ru', 'WORK'); //Добавляем рабочую почту
    $contact->delEmail('test@test.ru'); //Удаляем почту
    $contact->setResponsibleUser('Пётр Иванович'); //Меняем ответственного
    $contact->save(); //Сохраняем все изменение на сервере црм
    $contact->addTask('Позвонить клиенту', 'Александр'); //Прикрепляем задачку, и назначаем ответственным за неё Александра
} else {
    echo 'что-то пошло не так';
}
?>
```

## Создание сделки с двумя привязанными контактами в "Неразобранном" в вороке "Вторые продажи"
```php
<?php
use AmoCRM\Amo;
use AmoCRM\Contact;
use AmoCRM\Lead;
use AmoCRM\Unsorted;
     
$amo = new Amo('test', 'test@test.ru', '011c2d7f862c688286b43ef552fb17f4');
if ($amo->isAuthorization()) {
    $contact = new Contact();
    $contact->setName('Петя');
    $contact->addPhone(79998887766); //Создаём контакт, который будет создан в црм после принятия заявки в неразобранном
    $contact2 = new Contact();
    $contact2->setName('Ваня');
    $contact2->addPhone(79998887755); //Создаём второй контакт, который будет создан в црм после принятия заявки в неразобранном
    $lead = new Lead();
    $lead->setName('Тестовая сделка');
    $lead->setPrice(9999); //Создаём сделку, которая будет создана в црм после принятия заявки в неразобранном
    $unsorted = new Unsorted('Форма на лендосе', array($contact, $contact2), $lead, 'Вторые продажи');
    $unsorted->save(); // Сохраняем всё в неразобранное в црм
} else {
    echo 'что-то пошло не так';
}
?>
```

## Реализация стандартной логики интеграции Ройстат и Амо

Плюсом небольшая фишка с дозаполнением данными контакта

```php
<?php
use AmoCRM\Amo;
use AmoCRM\Contact;
use AmoCRM\Lead;

//Тестовые данные
$form = 'Заказать звонок';
$name = 'Тест';
$phone = '+7(999)888-77-66';
$email = 'testik@test.ru';
$responsibleUserId = 'Александр';
$comment = 'Нужно быстрей';

$amo = new Amo('test', 'test@test.com', '8a66666666b3494179da07abc74bfd49');
if ($amo->isAuthorization()) {
    $lead = new Lead();
    $lead->setName("Заявка с формы '$form'");
    $lead->setCustomField('roistat', isset($_COOKIE['roistat_visit']) ? $_COOKIE['roistat_visit'] : null);
    $lead->setCustomField('roistat-marker', isset($_COOKIE['roistat_marker']) ? $_COOKIE['roistat_marker'] : 'Прямой визит');
    $lead->setCustomField('Форма захвата', $form);
    $lead->setCustomField('utm_source', $_COOKIE['utm_source']);
    $lead->setCustomField('utm_medium', $_COOKIE['utm_medium']);
    $lead->setCustomField('utm_campaign', $_COOKIE['utm_campaign']);
    $lead->setCustomField('utm_term', $_COOKIE['utm_term']);
    $lead->setCustomField('utm_content', $_COOKIE['utm_content']);
    $lead->setResponsibleUser($responsibleUserId);
    $isSaveLead = $lead->save();
    $lead->addNote($comment);
    $contacts = $amo->searchContact($phone, $email);
    if (!empty($contacts)) {
        /** @var Contact $contact */
        $contact = current($contacts);
    } else {
        $contact = new Contact();
        $contact->setName($name);
    }
    $contact->addPhone($phone);
    $contact->addEmail($email);
    if ($isSaveLead) {
        $contact->addLinkedLeadId($lead->getId());
    }
    $contact->save();
}
?>
```