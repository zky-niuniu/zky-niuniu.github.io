最佳超参数训练

寻优代码：

```python
import warnings
warnings.filterwarnings('ignore')
from ultralytics import YOLO
if __name__ == '__main__':
    model = YOLO('/data/coding/runs/detect/yolov8s_eucalyptus_advanced/weights/best.pt')      # 需要修改
    model.load('yolov8n.pt') # loading pretrain weights
    model.tune(data=r'/data/coding/rec/data.yaml',
                epochs=50,
                batch=32,
                close_mosaic=10,
                device='0',
                optimizer="SGD",
                workers=8,# 数据加载线程数（建议=CPU核心数）
                pretrained=True,# 启用预训练权重加速收敛
                project='runs/train',
                iterations=15,
                name='exp',
                plots=False,
                )
```

训练代码：

```python
import warnings
warnings.filterwarnings('ignore')
from ultralytics import YOLO
 
# 加载模型
model = YOLO('yolov8m.pt')
# 开始训练任务
model.train(data="/data/coding/rec/data.yaml",
            epochs=200, 
            batch=16, 
            imgsz=640,
            name='yolov8s_eucalyptus_advanced',
            lr0= 0.01066,
            lrf= 0.00988,
            momentum= 0.88938,
            weight_decay= 0.00045,
            warmup_epochs= 3.63717,
            warmup_momentum= 0.88001,
            box= 7.8841,
            cls= 0.59704,
            dfl= 1.52442,
            hsv_h= 0.01474,
            hsv_s= 0.5951,
            hsv_v= 0.40185,
            degrees= 0.0,
            translate= 0.10077,
            scale= 0.63505,
            shear= 0.0,
            perspective= 0.0,
            flipud= 0.0,
            fliplr= 0.44332,
            bgr= 0.0,
            mosaic= 0.98881,
            mixup= 0.0,
            copy_paste= 0.0
            )
```

原始的训练

```python
import warnings
warnings.filterwarnings('ignore')
from ultralytics import YOLO
 
# 加载模型
model = YOLO('yolov8m.pt')
# 开始训练任务
model.train(data="/data/coding/rec/data.yaml",
            epochs=200, 
            batch=16, 
            imgsz=640, 
            lr0=0.005, 
            device=0,
            weight_decay=0.0005,
            name='yolov8s_eucalyptus_advanced',
            # 可以使用focal loss处理类别不平衡
            augment=True,  # 启用数据增强
            #fl_loss=1.5,  # focal loss gamma参数
            # 增加分类损失的权重，帮助更好区分背景
            cls=1,
            box=0.08,
            iou=0.45,
            conf=0.5,
            hsv_h=0.1,  # 色调增强
            hsv_s=0.7,  # 饱和度增强
            hsv_v=0.4,  # 明度增强
            degrees=15.0,  # 旋转角度范围
            translate=0.1,  # 平移范围
            scale=0.5,  # 缩放范围
            shear=2.0,  # 剪切范围
            flipud=0.5,  # 上下翻转概率
            fliplr=0.5,  # 左右翻转概率
            mosaic=1.0,  # mosaic数据增强概率
            mixup=0.1,  # mixup数据增强概率
            copy_paste=0.5  # 随机粘贴背景
            )

```

注意力机制训练

```python
import warnings
warnings.filterwarnings('ignore')
from ultralytics import YOLO
# 加载预训练模型
# 添加注意力机制，SEAtt_yolov8.yaml 默认使用的是n。
# SEAtt_yolov8s.yaml，则使用的是s，模型。
model = YOLO("ultralytics/cfg/models/v8/SEAtt_yolov8.yaml").load('yolov8n.pt')
# Use the model
if __name__ == '__main__':
    # Use the model
    results = model.train(data='/data/coding/ultralytics/data.yaml', 
    epochs=200, 
    batch=16,
    name='attention',
    imgsz=640, 
    # 超参数示例（部分）
    lr0=3e-4,  # 初始学习率
    lrf=0.01,   # 最终学习率（余弦退火）
    weight_decay=0.0005,
    warmup_epochs=5, 
    iou=0.45,
    augment=True,  # 启用默认增强（Mosaic/MixUp等）
    hsv_h=0.015,  # 色调增强强度
    hsv_s=0.7,    # 饱和度增强强度
    hsv_v=0.4,    # 亮度增强强度
    box=7.0,  # 框回归损失权重
    cls=1.5,  # 分类损失权重（可增至1.5抑制误检）
    )  # 训练模型
```

主干网络替换

```python
import warnings
warnings.filterwarnings('ignore')
from ultralytics import YOLO
if __name__ == '__main__':
    model = YOLO('/data/coding/ultralytics/ultralytics/cfg/models/v8/yolov8-FasterNet.yaml')
    model.load('yolov8n.pt') # loading pretrain weights
    model.train(data='/data/coding/ultralytics/data.yaml', 
                epochs=300, 
                batch=16,
                patience=20,
                name='main',
                # 超参数示例（部分）
                lr0=3e-4,  # 初始学习率
                lrf=0.01,   # 最终学习率（余弦退火）
                weight_decay=0.0005,
                warmup_epochs=5, 
                iou=0.45,
                augment=True,  # 启用默认增强（Mosaic/MixUp等）
                hsv_h=0.015,  # 色调增强强度
                hsv_s=0.7,    # 饱和度增强强度
                hsv_v=0.4,    # 亮度增强强度
                box=7.0,  # 框回归损失权重
                cls=1.5,  # 分类损失权重（可增至1.5抑制误检）
                )
```

