# 获取数据集

## 一、爬取照片

如下代码：

```python
import re
import requests

headers = {
    "User-Agent": 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36 Edg/133.0.0.0',
    "Cookie": "BDqhfp=%E4%BA%95%E7%9B%96%26%26NaN%26%260%26%261; BAIDUID=C485C263EDCE26F7DE6CD800051C921A:FG=1; PSTM=1717060843; BIDUPSID=C1B15792C8A3E1F70394B9FBE864D684; BAIDUID_BFESS=C485C263EDCE26F7DE6CD800051C921A:FG=1; BAIDU_WISE_UID=wapp_1732703502376_96; H_WISE_SIDS_BFESS=61027_61079_61135_61140_61161_61217_61211_61213_61208; BDUSS=VAzVUpadHQ1MFBNTkNFUDRQdGZNR0w0cUphSlJoMDVHYXFVQXlifjhrWUNwSXRuSVFBQUFBJCQAAAAAAQAAAAEAAABqb1Z7eHNiMjM0NTYAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIXZGcCF2RnS; BDUSS_BFESS=VAzVUpadHQ1MFBNTkNFUDRQdGZNR0w0cUphSlJoMDVHYXFVQXlifjhrWUNwSXRuSVFBQUFBJCQAAAAAAQAAAAEAAABqb1Z7eHNiMjM0NTYAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIXZGcCF2RnS; ZFY=73qcm:B8K:ALF8SwaVRIqq0iuZq056:Ajhfogz8eVdoyQE:C; __bid_n=195372609c7436814fafe9; ai-studio-ticket=D021C61067DE45A88B474EBEA3D045519E4F7E6AACD543FB9F8EEA66EBB40210; jsdk-uuid=bc921263-a863-43ff-b4c0-81161dd4be46; H_PS_PSSID=61027_62079_62066_62128_62168_62185_62187_62180_62197_62234_62232_62281_62136; BA_HECTOR=00ag2k8g250k0g8k8g8hag001i9l511jrr8ij1v; BDRCVFR[feWj1Vr5u3D]=I67x6TjHwwYf0; PSINO=3; delPer=0; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; H_WISE_SIDS=62185_62187_62180_62197_62232_62281; BDRCVFR[dG2JNJb_ajR]=mk3SLVN4HKm; BDRCVFR[-pGxjrCMryR]=mk3SLVN4HKm; arialoadData=false; cleanHistoryStatus=0; userFrom=null; RT=z=1&dm=baidu.com&si=b376c241-8c18-4e7f-a296-d7a9ba359c15&ss=m7kgimdx&sl=6&tt=4iy&bcn=https%3A%2F%2Ffclog.baidu.com%2Flog%2Fweirwood%3Ftype%3Dperf&ld=64bc; BDRCVFR[Txj84yDU4nc]=mk3SLVN4HKm; indexPageSugList=%5B%22%E4%BA%95%E7%9B%96%22%2C%22%E6%88%B7%E5%A4%96%E4%BA%95%E7%9B%96%22%5D; ab_sr=1.0.1_YjJjYzRmOTMwNDhhMzljOTY3ODUwMWI1NGQ0NTE0NWJmMDUzZWY1ZGRhYWVjYTVkMzkzZTE5ZWJkZTdhMzVkOTgyOTBjMjc4MzgwYzkxNGM4MzczM2Q3ZWQxYzU1ZmM0NjhmNmVlMmYwOGFhODcyNTcxNWMwMzBhYmFiNTE3YmExNzAxY2YzYWZhNjNlMWJmZTE3ZTk4YTBjMDI2MjU4Nw==",
}

detail_urls = []  # 存储图片地址

for i in range(1, 401, 20):  # 注意结束范围是401以确保包含最后一页（如果每页20个结果）
    url = 'https://image.baidu.com/search/flip?tn=baiduimage&ipn=r&ct=201326592&cl=2&lm=&st=-1&fm=result&fr=&sf=1&fmq=1740486678338_R&pv=&ic=&nc=1&z=&hd=&latest=&copyright=&se=1&showtab=0&fb=0&width=&height=&face=0&istype=2&dyTabStr=&ie=utf-8&ctd=1740486678339%5E00_1473X738&sid=&word=%E4%BA%95%E7%9B%96'.format(i)  # （省略了URL的其余部分，保持原样）
    response = requests.get(url, headers=headers, timeout=(3, 7))
    response.raise_for_status()  # 检查请求是否成功
    content = response.text  # 直接使用text，因为您正在搜索HTML中的字符串
    detail_urls.extend(re.findall(r'"objURL":"(.*?)"', content, re.DOTALL))  # 使用extend来合并列表

# 重置图片计数器
b = 0
for url in detail_urls:
    try:
        b += 1  # 在尝试下载之前增加计数器
        print(f'正在获取第{b}张图片...')
        response = requests.get(url, headers=headers, timeout=(3, 7))
        response.raise_for_status()  # 再次检查请求是否成功
        file_extension = url.split('.')[-1]  # 获取文件扩展名（注意：这可能不是最安全的方法，因为URL中可能包含点）
        if file_extension in ['jpg', 'jpeg', 'png']:  # 检查有效的文件扩展名
            with open(f'保存的地址{b}.{file_extension}', 'wb') as f:
                f.write(response.content)
        else:
            print(f'跳过未知扩展名的图片：{url}')
    except requests.exceptions.RequestException as e:
        print(f'请求失败：{e}')

```

## 二、数据分类标注

```
pip install pyqt5 # 安装图形化依赖包

pip install labelImg 
```

打开cmd窗口即可使用：

```cmd
labelimg
```

进入图形化界面，选择yolo格式

三、数据分类

将数据分为训练测试验证三个集，代码如下：

```python
import os
import shutil
import random
 
# random.seed(0)  #随机种子，可自选开启
def split_data(file_path, label_path, new_file_path, train_rate, val_rate, test_rate):
	images = os.listdir(file_path)
	labels = os.listdir(label_path)
	images_no_ext = {os.path.splitext(image)[0]: image for image in images}
	labels_no_ext = {os.path.splitext(label)[0]: label for label in labels}
	matched_data = [(img, images_no_ext[img], labels_no_ext[img]) for img in images_no_ext if img in labels_no_ext]
 
	unmatched_images = [img for img in images_no_ext if img not in labels_no_ext]
	unmatched_labels = [label for label in labels_no_ext if label not in images_no_ext]
	if unmatched_images:
		print("未匹配的图片文件:")
		for img in unmatched_images:
			print(images_no_ext[img])
	if unmatched_labels:
		print("未匹配的标签文件:")
		for label in unmatched_labels:
			print(labels_no_ext[label])
 
	random.shuffle(matched_data)
	total = len(matched_data)
	train_data = matched_data[:int(train_rate * total)]
	val_data = matched_data[int(train_rate * total):int((train_rate + val_rate) * total)]
	test_data = matched_data[int((train_rate + val_rate) * total):]
 
	# 处理训练集
	for img_name, img_file, label_file in train_data:
		old_img_path = os.path.join(file_path, img_file)
		old_label_path = os.path.join(label_path, label_file)
		new_img_dir = os.path.join(new_file_path, 'train', 'images')
		new_label_dir = os.path.join(new_file_path, 'train', 'labels')
		os.makedirs(new_img_dir, exist_ok=True)
		os.makedirs(new_label_dir, exist_ok=True)
		shutil.copy(old_img_path, os.path.join(new_img_dir, img_file))
		shutil.copy(old_label_path, os.path.join(new_label_dir, label_file))
	# 处理验证集
	for img_name, img_file, label_file in val_data:
		old_img_path = os.path.join(file_path, img_file)
		old_label_path = os.path.join(label_path, label_file)
		new_img_dir = os.path.join(new_file_path, 'val', 'images')
		new_label_dir = os.path.join(new_file_path, 'val', 'labels')
		os.makedirs(new_img_dir, exist_ok=True)
		os.makedirs(new_label_dir, exist_ok=True)
		shutil.copy(old_img_path, os.path.join(new_img_dir, img_file))
		shutil.copy(old_label_path, os.path.join(new_label_dir, label_file))
	# 处理测试集
	for img_name, img_file, label_file in test_data:
		old_img_path = os.path.join(file_path, img_file)
		old_label_path = os.path.join(label_path, label_file)
		new_img_dir = os.path.join(new_file_path, 'test', 'images')
		new_label_dir = os.path.join(new_file_path, 'test', 'labels')
		os.makedirs(new_img_dir, exist_ok=True)
		os.makedirs(new_label_dir, exist_ok=True)
		shutil.copy(old_img_path, os.path.join(new_img_dir, img_file))
		shutil.copy(old_label_path, os.path.join(new_label_dir, label_file))
	print("数据集已划分完成")
 
if __name__ == '__main__':
	file_path = r"/Volumes/Game/data/train/JPEGImages"  # 图片文件夹
	label_path = r'/Volumes/Game/data/train/Anotations_TXT'  # 标签文件夹
	new_file_path = r"/Volumes/Game/data_train"  # 新数据存放位置
	split_data(file_path, label_path, new_file_path, train_rate=0.8, val_rate=0.1, test_rate=0.1)
```