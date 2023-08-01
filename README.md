# LetaiRacing 2023
Информационные материалы для подготовки к соревнованию LetaiRacing 2023

## О мероприятии
Соревнование [LetaiRacing 2023](https://letai.ru/letai-racing/) в категории JetRacer 
проводится в рамках Международной выставки Kazan Digital Week.

[Регламент сореввнований](https://docs.google.com/document/d/1yJ5hXllcJqLbtUS8Hy3ZLYWD6tC2-FXb0se56mZp0p4/edit)

## Начало работы
### Сборка машинки
Производится по [инструкции](https://www.waveshare.com/w/upload/f/fa/Jetracer_pro_Assembly_EN.pdf) в коробке машинки

### Установка ПО
Инструкция от [waveshare](https://www.waveshare.com/wiki/JetRacer_Pro_AI_Kit#Introduction)\
Инструкция от [NVIDIA](https://github.com/NVIDIA-AI-IOT/jetracer/blob/master/docs/software_setup.md#latest-release--but-not-yet-fully-verified--) (тут ссылка на ПО поновее и меньше размером)\
[Видео мануал](https://www.youtube.com/watch?v=cf30ZQWauI8)

### Быстрый старт
Подходы к решению
1. Использование библиотеки [jetracer](https://github.com/NVIDIA-AI-IOT/jetracer)
2. Использование библиотеки [DonkeyCar](https://docs.donkeycar.com/)
3. Прочие подходы 

### Использование библиотеки [DonkeyCar](https://docs.donkeycar.com/)
#### Установка Donkey Car на jetracer

Ниже представлены списки команд, которые необходимо выполнить в консоли:

#### Удаляем Libre Office (опционально, офис там не нужен):
```bash
sudo apt-get remove --purge libreoffice*
sudo apt-get clean
sudo apt-get autoremove
```

#### Увеличиваем файл подкачки (опционально, на случай если будет не хватать):
```bash
git clone https://github.com/JetsonHacksNano/installSwapfile
cd installSwapfile
./installSwapfile.sh
sudo reboot now 
```

#### Устанавливаем общесистемные зависимости:
```bash
sudo apt-get install -y libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev liblapack-dev libblas-dev gfortran
sudo apt-get install -y python3-dev python3-pip
sudo apt-get install -y libxslt1-dev libxml2-dev libffi-dev libcurl4-openssl-dev libssl-dev libpng-dev libopenblas-dev
sudo apt-get install -y git nano
sudo apt-get install -y openmpi-doc openmpi-bin libopenmpi-dev libopenblas-dev
```

#### Настраиваем среду Python:
```bash
pip3 install virtualenv
python3 -m virtualenv -p python3 env --system-site-packages
echo "source ~/env/bin/activate" >> ~/.bashrc
source ~/.bashrc
```

#### Настраиваем зависимости Python:
```bash
pip3 install -U pip testresources setuptools
pip3 install -U futures==3.1.1 protobuf==3.12.2 pybind11==2.5.0
pip3 install -U cython==0.29.21 pyserial
pip3 install -U future==0.18.2 mock==4.0.2 h5py==2.10.0 keras_preprocessing==1.1.2 keras_applications==1.0.8 gast==0.3.3
pip3 install -U absl-py==0.9.0 py-cpuinfo==7.0.0 psutil==5.7.2 portpicker==1.3.1 six requests==2.24.0 astor==0.8.1 termcolor==1.1.0 wrapt==1.12.1 google-pasta==0.2.0
pip3 install -U gdown

# This will install tensorflow as a system package
pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v45 tensorflow==2.3.1
```

#### Устанавливаем библиотеку Donkeycar:
```bash
mkdir projects
cd ~/projects
git clone https://github.com/autorope/donkeycar
cd donkeycar
git fetch --all --tags -f
latestTag=$(git describe --tags `git rev-list --tags --max-count=1`)
git checkout $latestTag
pip install -e .[nano45]
```
#### Кабибровка сервопривода и мотора

[Инструкция по калибровке](https://docs.donkeycar.com/guide/calibrate/), подаваемые на ШИМ (PCA9685), 
чтобы найти полный ход вперед, полный назад, максимальные повороты в стороны.
```bash
donkey calibrate --pwm-pin PCA9685.1:40.1
donkey calibrate --pwm-pin PCA9685.1:40.0
```
Для первоначальной настройки можно использовать как ориентир:

```
    "STEERING_LEFT_PWM": 460,               #pwm value for full left steering
    "STEERING_RIGHT_PWM": 320,              #pwm value for full right steering
    "THROTTLE_FORWARD_PWM": 480,            #pwm value for max forward throttle
    "THROTTLE_STOPPED_PWM": 337,            #pwm value for no movement
    "THROTTLE_REVERSE_PWM": 140,            #pwm value for max reverse throttle
````
#### Примечания
При обучении на ПК **myconfig.py** должен быть такой же как и на jetracer


## Ознакомительные материалы
1. [JetRacer AI Kit](https://www.waveshare.com/wiki/JetRacer_AI_Kit)
2. [NVIDIA-AI-IOT/jetracer](https://github.com/NVIDIA-AI-IOT/jetracer)
3. [Jetson AI Fundamentals - S1E2 - Hello Camera](https://www.youtube.com/watch?v=zsjcSapzUfU&list=PL5B692fm6--uQRRDTPsJDp4o0xbzkoyf8&index=15)
4. [Get Your Jetson Nano/Xavier NX Working](http://docs.donkeycar.com/guide/robot_sbc/setup_jetson_nano/)
5. [Self-driving ГАЗ66 Monster Truck 1/16](https://habr.com/ru/articles/489046/)
6. [Дорожный датасет](https://github.com/Ezward/donkey_datasets/tree/master)
7. [Моделька для детектирования препятсивий](https://universe.roboflow.com/jimmyaiot/donkey-car/model/1)
8. [Donkey Car Training using Google Colab](https://colab.research.google.com/github/robocarstore/donkey-car-training-on-google-colab/blob/master/Donkey_Car_Training_using_Google_Colab.ipynb)
9. [Donkey Car Training  modified using Google Colab](https://colab.research.google.com/github/uwesterr/MlFastAiBlog/blob/master/_notebooks/2020-03-01-TrainDonkeyCar.ipynb#scrollTo=4mVN5ljz8l_3)
10. [Симуляция гонок в Unity](https://flyyufelix.github.io/2018/09/11/donkey-rl-simulation.html)
11. [Песочница для самоуправляемых автомобилей](https://github.com/tawnkramer/sdsandbox)
12. [Train Donkey Car in Unity Simulator with Reinforcement Learning](https://github.com/flyyufelix/donkey_rl)
13. [Train an Autopilot](https://docs.donkeycar.com/guide/train_autopilot/)
14. [УСТАНОВКА DONKEYCAR: СИМУЛЯТОР](https://ori.codes/software/donkeycar-simulator/)
15. [OpenAI Gym Environments for Donkey Car](https://github.com/tawnkramer/gym-donkeycar)
16. [Donkey Simulator](http://docs.kittcar.com/donkey_sim/)
17. [balena-DonkeyCar-Simulator](https://github.com/dtischler/balena-DonkeyCar-Simulator)
18. [Donkey Simulator](https://docs.donkeycar.com/guide/deep_learning/simulator/)
19. [DonkeyCar Simulator: Setup and Reinforcement Learning Training (Part 1)](https://www.youtube.com/watch?v=ngK33h00iBE&t=4403s)
20. [DonkeyCar Simulator: Autoencoder training (Part 2)](https://www.youtube.com/watch?v=DUqssFvcSOY&t=808s)
