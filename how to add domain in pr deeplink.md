## Предположим, уже была проделана подготовка по подключению диплинков
нужные ссылки 
+ https://codewithandrea.com/articles/flutter-deep-links/
+ ios - https://docs.flutter.dev/cookbook/navigation/set-up-universal-links
+ andr - https://habr.com/ru/articles/837330/
+ andr - https://stackoverflow.com/questions/70143836/android-app-link-not-working-in-android-12-always-opening-in-browser
### Андроид: 
+ 1 в android/app/src/main/AndroidManifest.xml дублируешь intent-filter с новым доменом
+ 2 на домейн нового сервера нужно залить новые Asset Links, если уже приложение уже зарегестрировано в гугл консоли, их можно найти в google play console -> setup -> app signing -> Digital Asset Links JSON
+ 3 после того, как создал файл ассетлинков, попроси бек или сам лично залей их таким образом, чтобы этот json файл можно было получить перейдя на https://yourdomain/.well-known/assetlinks.json
+ 3* если хочешь проверить, что все сделал правильно, перейди на https://developers.google.com/digital-asset-links/tools/generator?hl=ru
+ 3** дополнительно, в гугл плей консоли есть раздел с диплинками, для аналитики и дополнительной надежности можно создать дополнительные диплинки
+ 4 если все сделано правильно, в сборках их гуглплея автоматически должны подключаться диплинки, а в дебаг сборках, перейдя в app info -> open by default должны появиться нужные домены для подключения(не подключив их, диплинки не будут работать)
+ 5 если хочешб проверить диплинки на эмуляторе, то можно использовать почту или ввести команду adb shell am start -W -a android.intent.action.VIEW -d "myapp://welcome" com.example.name, где welcome - это путь, который будет обрабатывать библиотека навигации, а com.example.name - это название package приложения искать в main activity.kt

### IOS:
+ 1 для начала перейди открой ios в xcode, перейди в Runner -> пункт signing & capabilities
+ 2 добавь в associated domains новый домэйн, по примеру другого, т.е applinks:yourDomain.com
+ 3 запуш apple-app-site-association, на тот же адресс, что и андроид, т.е он должен получаться по тому же адресу: https://yourdomain/.well-known/apple-app-site-association. Главное различие в том, что файл не должен содержать какой либо формат
+ 3* если не осталось файла, который нужно разместить в домэйне, можно взять универсальный файл из официальной [доки](https://docs.flutter.dev/cookbook/navigation/set-up-universal-links), единственное нужно знать:
++ team id - получаешь от владельца команды разработчиков по ссылке https://developer.apple.com/account
++ bundle id - заходишь в app store connect -> выбирешь свою команду -> выбираешь свое приложение -> после этого ты должен попасть на вкладку распространение -> общая инофрмация, инофрмация о приложении -> ID пакета
