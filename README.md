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

https://docs.ros.org/en/iron/Installation/Ubuntu-Install-Debians.html

1. Проверим ....
```bash
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings
```
2. You will need to add the ROS 2 apt repository to your system.

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

Now add the ROS 2 GPG key with apt.

```bash
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

Then add the repository to your sources list.

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

```bash
sudo apt update && sudo apt install ros-dev-tools
```

```bash
sudo apt update
```

```bash
sudo apt upgrade
```

```bash
sudo apt install ros-iron-desktop
```

окружение
```bash
source /opt/ros/iron/setup.bash
```
Для отправителя
```bash
source /opt/ros/iron/setup.bash
ros2 run demo_nodes_cpp talker
```
Для слушателя
```bash
source /opt/ros/iron/setup.bash
ros2 run demo_nodes_py listener
```