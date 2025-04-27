最佳超参数训练

寻优代码：

```python
import warnings
warnings.filterwarnings('ignore')
from ultralytics import YOLO
if __name__ == '__main__':
	model = YOLO('/data/coding/runs/detect/train11/weights/best.pt') # 需要修改
	model.load('yolov8n.pt') # loading pretrain weights
	model.tune(data=r'/data/coding/rec/data.yaml',
	epochs=50,
	#single_cls=False, # 是否是单类别检测
	batch=16,
	close_mosaic=10,
	device='0',
	optimizer='SGD',
	project='runs/train',
	iterations=100,
	name='exp',
	)
```

训练代码：

```python
import warnings
warnings.filterwarnings('ignore')
from ultralytics import YOLO
if __name__ == '__main__':
   model=YOLO('/data/coding/yolov8n.pt')  #加载预训练权重
   results = model.train(data='/data/coding/ultralytics/data.yaml',   #数据集yaml文件
               imgsz=640,
               epochs=200,
               batch=16,
               workers=0,  
               device=0,   #没显卡则将0修改为'cpu'
               optimizer='SGD',
               close_mosaic=0,
               plots=True,
               cache=True,   #服务器可设置为True，训练速度变快
               name='super'
)
```

原始的训练

```python
import warnings
warnings.filterwarnings('ignore')
from ultralytics import YOLO
if __name__ == '__main__':
  model=YOLO('/data/coding/yolov8n.pt') #加载预训练权重
  results = model.train(data='/data/coding/ultralytics/data.yaml', #数据集yaml文件
  imgsz=640,
  epochs=200,
  batch=16,
  workers=0,
  device=0, #没显卡则将0修改为'cpu'
  optimizer='SGD',
  close_mosaic=0,
  plots=True,
  cache=True, #服务器可设置为True，训练速度变快
  name='original'
  )
```

注意力机制训练

```python
import warnings
warnings.filterwarnings('ignore')
from ultralytics import YOLO
# 加载预训练模型
# 添加注意力机制，SEAtt_yolov8.yaml 默认使用的是n。
# SEAtt_yolov8.yaml
model = YOLO("ultralytics/cfg/models/v8/SEAtt_yolov8.yaml").load('yolov8n.pt')
# Use the model
if __name__ == '__main__':
    # Use the model
    results = model.train(data='/data/coding/ultralytics/data.yaml', 
  	epochs=200, 
  	batch=200,
   	name='attention')  # 训练模型
# 验证
print(metrics.box.map)  # 打印mAP50-95
# 可视化一些验证结果
model.val(save_json=True, save_hybrid=True)
```

