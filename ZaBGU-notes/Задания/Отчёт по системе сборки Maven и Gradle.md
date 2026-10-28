**Apache Maven** — это инструмент для автоматизации сборки проектов, управления зависимостями и жизненным циклом программ на языке Java.
	
**Gradle** — это мощная современная система автоматической сборки проектов, которая чаще всего используется в экосистеме Java (а также Kotlin, Android, Groovy и др.).

Я буду использовать свой maven, не встроенный.

---
# Установка Maven:
1) https://maven.apache.org/download.cgi#CurrentMaven - переходим по ссылке и скачиваем zip файл
2) Распаковываем архив (например в C:\Program Files\Apache\Maven)
3) Добавляем в переменные среды: 
MAVEN_HOME
![[Pasted image 20261028011215.png|402]]

И в path:
![[Pasted image 20261028011317.png|239]]

4) Теперь можем проверить версию maven при помощи команды `mvn -version`

![[Pasted image 20261028011635.png]]

5) В настройках Intellij Idea в поиске вводим: `Build, Execution, Deployment → Build Tools → Maven`
6) В поле `Maven home path` указываем наш maven
	IntelliJ IDEA поставляется со встроенным Maven (Bundled). Можно использовать его - тогда ничего указывать не нужно.

![[Pasted image 20261028013142.png|642]]

---
# Создание проекта на maven

1)  Создаём новый проект и выбираем `Maven`
![[idea64_vTqQA0aHkE 1.png|411]]
	Также проект можно создать через архетип (шаблон проекта, который позволяет быстро создавать готовую структуру папок и базовую конфигурацию):

```shell
mvn archetype:generate -DgroupId=com.mycompany -DartifactId=myapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```


Наш проект имеет такую структуру:
![[Pasted image 20261028014621.png|265]]
- Основной код (Main.java) находится в директории src\main\java\org.example;
- **pom.xml** - Файл конфигурации Maven (Project Object Model): координаты проекта (`groupId`, `artifactId`, `version`), зависимости, плагины, версия Java;
- src/test/java/ - Тесты (JUnit); не попадают в итоговый jar
- src/main/resources/ - (создаётся вручную при необходимости) ресурсы: конфиги, properties, шаблоны — попадают в classpath
- src/test/resources/ - ресурсы для тестов
- target/ - (появляется после сборки) скомпилированные классы, jar, отчёты — **не коммитится** в git

---
# Компиляция и запуск
1) В папке с нашим кодом открываем терминал и вводим команду `mvn compile`
![[Pasted image 20261028020024.png]]
После ввода ожидаем надпись `BUILD SUCCESS`:
![[Pasted image 20261028020132.png|669]]

Также в проекте появились новые папки (например target\classes\com\mycompany\App.class):
![[Pasted image 20261028020229.png|235]]

2) Вводим команду запуска проекта и смотрим на вывод:
![[Pasted image 20261028021424.png|629]]

Всё работает. А теперь перенесём нашу самую первую программу в проект maven. Копируем наши java файлы в проект, заменяем имя пакетов, компилируем и запускаем:
![[Pasted image 20261028022133.png|669]]

---
# Собираем исполняемый файл jar
`clean` — это **отдельная цель (goal)**, которая просто **удаляет папку `target/`** проекта. Всё, что было собрано раньше, стирается, и следующая сборка начинается с чистого листа.
Может убрать ошибки, которых нет в коде, устаревшие .class и т.д.
![[Pasted image 20261028023248.png|660]]

![[Pasted image 20261028023519.png]]
Появляется файл с расширением .jar, затем запускаем его:
![[Pasted image 20261028023608.png|639]]
**JAR-файл** — это Java-архив (сокращение от англ. Java ARchive). Представляет собой ZIP-архив, в котором содержится часть программы на языке Java.

Использованные источники:
1) https://skillbox.ru/media/code/osnovy-maven-chto-eto-takoe-i-kak-rabotaet/ // maven
2) https://blog.skillfactory.ru/glossary/gradle/ // gradle
3) https://ru.wikipedia.org/wiki/JAR // jar