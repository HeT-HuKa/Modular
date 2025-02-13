# Модули
## Модуль А
### Подключение робота
1. Настройка wifi
    - Переходим в директорию на CD карте
    ```
    /etc/wpa_supplicant/wpa_supplicant.conf
    ```
    - Редактируем файл wpa_supplicant.conf
    ```
    network={
        ssid="WIFI_NETWORK_NAME"
        psk="wifipassword"
    }
    ```
    - Редактируем ssid (название сети) и psk (пароль сети)
2. Изменение имени робота
    - Переходим в директорию
    ```
    /etc
    ```
    - В ней находим hostname и редактирем его
    ```
    sudo nano hostname
    ```
    - Изменяем turelbroXX на номер своего робота
3. Если все сделано успешно пытаемся подключиться
    ```
    ssh pi@192.168.1.XX - где XX Ip робота
    или
    ssh pi@turtelbroXX.local
    ```
### Внесение данных в "Акт о приеме"
1. Название дистрибутива Linux и кодовое имя сборки Linux
    ```
    lsb_release -a
    ```
2. Версия библиотеки rospy
    ```
    rosservice rospy
    ```
3. Размер оперативной памяти (Мбайт)
    ```
    free -M
    ```
4. Допустимый диапазон частот подключения робота к сети 5 ГГц
    ```
    iwconfig
    ```
5. Что бы узнать напряжение из топика
    ```
    rostopic echo /bat
    ```
6. IMU датчик работает корректно
    ```
    rostopic echo /imu
    ```
    - Смотрим на показатель *W*
7. Для проверки кнопок D22-D25 переходим по
      [ссылке](https://github.com/voltbro/ws-sro/tree/main/Turtlebro-tester)
    - Переходим в скетч Turtlebro-tester.ino
    - Копируем его и вставляем в Arduino
### Внесение данных в "Инструкция по вводу робота в эксплуатацию"
1. Первая часть
    - Присвоенное имя робота в сети
    ```
    hostname
    ```
    - IP-адрес робота в сети роутера-полигона
    ```
    ifconfig
    ```
    - Текущая частота подключения робота к сети 5 ГГц
    ```
    iwconfig
    Для этого нужно быть подключенным к сети TurtleBro5G
    ```
    - Текущая частота подключения робота к сети 2.4 ГГц
    ```
    iwconfig
    Для этого нужно быть подключенным к сети TurtleBro
    ```
2. Вторая часть
    - Название дистрибутива Linux и кодовое имя Linux
    ```
    lsb_release -a
    ```
    - Версия интерпретатора Python3
    ```
    python3 -V
    ```
    - Версия библиотеки rospy
    ```
    rosversion rospy
    ```
    - Версия пакета turtlebro
    ```
    rosversion turtlebro
    ```
    - Версия прошивки микроконтроллера материнской платы
    ```
    uname -a
    После Linux Username будут цифры - это и есть версия
    ```
    - Размер оперативной памяти (Мбайт)
    ```
    free -M
    ```
    - Текущий часовой пояс на роботе в формате “Time zone:Continent/City (XXX, +XXXX)”
    ```
    timedatectl | grep "Time zone"
    ```
    - Серийный номер Raspberry Pi 4
    ```
    cat /proc/cpuinfo | grep Serial
    ```
    - Топики из инструкции к роботу присутствуют на роботе
    ```
    rostopic list
    ```
3. Третья часть
    - Температура процессора в градусах (С)
    ```
    cat 
    ```
    - Текущее разрешение камеры (пикселей)
    ```
    v4l-ctl --all
    ```
    - Значение напряжения аккумуляторной сборки из топика батареи
    ```
    rostopic echo /bat
    ```
4. Четвертая часть
    - Камера работоспособна
    ```
    Переходим по:
    192.168.1.XX:8008
    ```
    - Одометрия корректна при линейном движении робота
    ```
    Для коррекного отображения значений нужно проехать по линейки определленное расстояние
    И сравнить его со значениями показанными в вед-интерфейсе (192.168.1.XX:8008)
    ```
    - Одометрия корректна при угловом вращении робота
    ```
     В левую сторону положительная, а в правую отрицательная
    ```
    - IMU датчик работает корректно
    ```
    rostopic echo /imu
    ```
    - Лидар работает корректно
    ```
    rostopic echo /scan
    ```
    - Стерео-акустическая система работает корректно
    ```
    echo "Однажды, в студеную зимнюю пору Я из лесу вышел. Был сильный мороз." | festival --tts --language russian
    ```
    - Сервопривод работает корректно [(ссылка)](https://github.com/HeT-HuKa/-/blob/main/Servo.ino)
    - Концевой выключатель работает корректно [(ссылка)](https://github.com/voltbro/ws-sro/blob/main/TB-limit_switch-tester/)
    - Нажимная кнопка работает корректно [(ссылка)](https://github.com/HeT-HuKa/-/blob/main/Button.ino)
    - Проверка светодиодной подсветки и кнопок D22-D25 [(ссылка)](https://github.com/voltbro/ws-sro/tree/main/Turtlebro-tester)
    - Связь контроллера расширения с ROS работает [(ссылка)](https://github.com/voltbro/ws-sro/tree/main/TB-ros-tester)
        - Проверяем его
        ```
        rostopic echo /arduino_connect_tester
        ```
## Модуль Б
### Обновление сервисного пакета
- Переходим в директорию src и клонируем пакет
    ```
    cd ~/catkin_ws/src/
    git clone https://github.com/voltbro/profi_service_pkg_1.git
    ```
- Выходим на одну директорию назад
    ```
    cd ~/catkin_ws/
    или
    cd ../
    ```
- Затем компилируем его
    ```
    catkin_make --pkg profi_service_pkg_1
    ```
- После этого его нужно запустить
    ```
    roslaunch profi_service_pkg_1 start_configure.launch
    ```
- Если все сделанно правильно можно проверить его с помощью команд:
    ```
    time
    tqdm
    os
    platform
    psutil
    ```
### Откат версии сервисного пакета
- Переходим в директорию src и клонируем пакет
    ```
    cd ~/catkin_ws/src
    git clone https://github.com/voltbro/profi_service_pkg_2.git
    ```
- Выходим на одну директорию назад
    ```
    cd ~/catkin_ws
    или
    cd ../
    ```
- Скомпилируйте пакет
    ```
    catkin_make
    ```
- Запустите файл
    ```
    roslaunch profi_service_pkg_2 start_configure.launch
    ```
- Для проверки нужно посмотреть топики
    ```
    /bat
    /scan
    /imu
    ```
- С момощью команды (?)
    ```
    rospy.loginfo echo
    ```
