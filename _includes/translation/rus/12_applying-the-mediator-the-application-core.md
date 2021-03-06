<!-- ### Использование медиатора: ядро приложения -->

В этой главе мы кратко пройдемся по некоторым зонам ответственности
медиатора, который играет роль ядра приложения. Но, для начала,
давайте разберемся, что он представляет собой в целом. 

Основная задача ядра — управлять **жизненным циклом** модулей. Когда ядро 
получает **интересное сообщение** от модулей, оно должно определить, как
приложение должно на это отреагировать, таким образом ядро определяет момент
**запуска** или **остановки** определенного модуля или набора модулей.

{:.message}
В идеальной ситуации, однажды запущенный модуль, должен функционировать
самостоятельно. В задачи ядра не входит принятие решений о том, как реагировать,
например, на событие `DOM ready` — в нашей архитектуре у модулей есть достаточно
возможностей для того, чтобы принимать такие решения самостоятельно.

Возможно, у вас вызовет удивление то обстоятельство, что модулям может
потребоваться остановка. Такое может произойти случае, если модуль вышел из строя,
либо если в работе модуля происходят серьезные ошибки. Решение об остановке
модуля может помочь предотвратить некорректную работу его методов.
Последующий перезапуск таких модулей должен помочь решить возникшие проблемы.
Цель здесь в минимизации негативных последствий для пользователя нашего приложения.

В дополнение, ядро должно быть в состоянии **добавлять или удалять** модули
не ломая ничего. Типичный пример — определенный набор функций может быть
недоступен на начальном этапе загрузки страницы, но эти функции могут быть
загружены динамически, на основе определенных действий со стороны пользователя.
Возвращаясь к нашему примеру с GMail, google может, по умолчанию, держать чат
в свернутом состоянии и загружать соответствующий ему модуль динамически,
только в том случае, если пользователь проявит интерес к использованию этой
части приложения. С точки зрения оптимизации производительности, это должно
дать заметный эффект.

Обслуживание ошибок должно также обрабатываться ядром приложения. В добавок
к сообщению об интересных событиях модули также должны сообщать и о любых ошибках
которые произошли в их работе. Ядро должно соответствующим образом реагировать
на эти ошибки (к примеру, останавливать модули, перезапускать их и тд). Это
важно, как часть слабо связанной архитектуры, позволяющей реализовать новый
или лучший подход к реализации уведомления пользователя об ошибках без ручного
изменения в каждом модуле. Используя механизм для публикации событий и подписки
на них в медиаторе мы сможем достичь этого.
