# anaconda+pycharm部署YOLOv8 on macos

*niuniu*

## 一、下载mac的包管理器homebrew

```batch
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## 二、anaconda安装

```batch
brew search anaconda
brew cask install anaconda
echo 'export PATH="/usr/local/anaconda3/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```
创建conda虚拟环境

```batch
conda create --name yolov8
conda activate yolov8
```
安装依赖环境：numpy、matplotlib、pandas、pillow、scikit-learn、scipy、torch、opencv-python等，一般都会有requirements.txt

## 三、YOLO下载

```batch
git clone git@github.com:ultralytics/ultralytics.git
cd ultralytics..
```

## 四、验证环境

```batch
yolo predict model=./model/yolov8n.pt source='https://ultralytics.com/images/bus.jpg'
```

