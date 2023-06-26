<h1 align="center">Установка окружения</h1>

<h2 align="center">Установка <code>Guest Addition</code></h2>

1. В `Ubuntu` открываем терминал и вводим команду:
```bash
sudo apt install dkms build-essential
```
2. Далее нажимаем на панели виртуальной машины `"Устройства"` и выбираем пункт `"Подключить образ диска Дополнений гостевой ОС..."`.
3. Появляется смонтированный образ, заходим в него, запускаем внутри терминал и пишем туда команду запуска:
```bash
./autorun.sh
```

4. Перезагружаем виртуальную машину.

---

<h2 align="center">Установка <code>ROS2</code></h2>


1. Настроим локаль и кодировку `UTF-8` в операционной системе Ubuntu:
```bash
locale  # проверяем наличие UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # проверяем настройки
```
2. Добавляем ROS2 в репозиторий нашей системы:

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

2. Добавляем ключ GPG для ROS2:
Now add the ROS 2 GPG key with apt.

```bash
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

3. Теперь добавим репозиторий в список источников:

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

4. Установим инструменты разработки ROS:
```bash
sudo apt update && sudo apt install ros-dev-tools
```

5. Обновим кеш репозитория apt после проведённых настроек и убедимся что наша система обновлена:
```bash
sudo apt update
```
```bash
sudo apt upgrade
```

6. Устанавливаем ROS2 в нашу систему:
```bash
sudo apt install ros-iron-desktop
```
7. Проверим что установка прошла успешно, открываем два терминала и вводим следущие команды:

* Для отправителя
```bash
source /opt/ros/iron/setup.bash
ros2 run demo_nodes_cpp talker
```
* Для слушателя
```bash
source /opt/ros/iron/setup.bash
ros2 run demo_nodes_py listener
```

Если слушатель принимает сообщения отправителя, значит установка прошла успешно.

---

<h2 align="center">Установка <code>Intel RealSense SDK 2.0</code></h2>

1. Запишем открытый ключ сервера в директорию `/etc/apt/keyrings`:

```bash
sudo mkdir -p /etc/apt/keyrings
curl -sSf https://librealsense.intel.com/Debian/librealsense.pgp | sudo tee /etc/apt/keyrings/librealsense.pgp > /dev/null
```

2. Убедимся что установлена поддержка apt HTTPS:

```bash
sudo apt-get install apt-transport-https
```

3. Добавим сервер в список репозиторив:

```bash
echo "deb [signed-by=/etc/apt/keyrings/librealsense.pgp] https://librealsense.intel.com/Debian/apt-repo `lsb_release -cs` main" | \
sudo tee /etc/apt/sources.list.d/librealsense.list
sudo apt-get update
```

4. Установим библиотеки `librealsense2-dkms` и `librealsense2-utils`:

```bash
sudo apt-get install librealsense2-dkms
```

```bash
sudo apt-get install librealsense2-utils
```

5. На всякий случай усановим пакеты для разработчика и откладки:

```bash
sudo apt-get install librealsense2-dev
```
```bash
sudo apt-get install librealsense2-dbg
```

Проверим, что всё работает с помощью команды `realsense-viewer`. Если в запусившимся окне появилась наша камера, значит всё прошло успешно. 

---

<h2 align="center">Установка <code>ROS2 wrapper</code></h2>

1. Создадим рабочее протранство для ROS2:

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src/
```

2. Скопируем последнюю версию ROS2 wrapper для Intel RealSense в `~/ros2_ws/src/`:

```bash
git clone https://github.com/IntelRealSense/realsense-ros.git -b ros2-development
cd ~/ros2_ws
```
3. Установим зависимости:

```bash
sudo apt-get install python3-rosdep -y
sudo rosdep init 
rosdep update 
rosdep install -i --from-path src --rosdistro $ROS_DISTRO --skip-keys=librealsense2 -y
```

4. Соберём наш проект:

```bash
colcon build
```

5. Настроим исходное окружение:

```bash
ROS_DISTRO=iron
source /opt/ros/$ROS_DISTRO/setup.bash
cd ~/ros2_ws
. install/local_setup.bash
```

6. Проверим что всё прошло успешно и запустим узел камеры:
```bash
ros2 run realsense2_camera realsense2_camera_node
```

Если данная команда не вернула ошибок и узел камеры находится в состоянии работы, значит установка прошла успешно.

---

<h2 align="center">Список источников</code></h2>

1. Установка ROS2 Iron – https://docs.ros.org/en/iron/Installation/Ubuntu-Install-Debians.html
2. Установка Intel RealSense SDK 2.0 – https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md#installing-the-packages
3. Установка ROS2 Wrapper – https://github.com/IntelRealSense/realsense-ros
