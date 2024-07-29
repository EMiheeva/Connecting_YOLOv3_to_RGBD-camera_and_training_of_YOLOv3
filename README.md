# Connecting_YOLOv3_to_RGBD-camera_and_training_of_YOLOv3
Программный код для подключения нейронной сети YOLOv3 к RGBD-камере и дальнейшее ее обучение на созданном датасете.  

На летней практике мне была поставлена задача подключить нейронную сеть YOLOv3 к RGBD-камере и обучить ее видеть класс "губка".  
(см. репозиторий https://github.com/rangelokk/Practica/wiki )  

Результат подключения:  
![image](https://github.com/user-attachments/assets/62b46351-6f28-48c3-9000-8f6abb24de22)  

Я работала в **операционной системе** `Linux`, работала с терминалом. Чтобы обучить нейронную сеть мне понадобилось сделать разметку изображений, которые составляют датасет, в **графическом интерфейсе** `Yolo_Mark`. После разметки создавались необходимые для обучения файлы конфигурации. Далее я использовала **фреймворк** `Darknet`, с помощью которого я и обучила(training) нейронную сеть YOLOv3 на созданном(пользовательском) датасете, используя предварительно обученные веса и полученные из `Yolo_Mark` файлы.  

Результат обучения:  

![image](https://github.com/user-attachments/assets/8d870dcc-7e89-48ca-b8a7-f6963d376c12)  

Архитектура обученной мною нейронной сети выглядит следующим образом:  
![image](https://github.com/user-attachments/assets/9a3f1c75-3441-49f8-bc7c-a9e239fb3c54)  
`conv` – (convolutation layer) слой свёртки в нейронной сети. Он используется для извлечения признаков из входных данных путём применения свёртки.  
`max` – (maximum pooling layer) слой пулинга. Он используется для уменьшения размерности признаковых карт, что помогает снизить вычислительную сложность и улучшить инвариантность к масштабированию и переносу.  
`route` – (reorganization layer) метка, указывающая на слой объединения. Этот слой используется для изменения формы данных.  
`BFLOPs` – (Billion Floating Point Operations Per Second) количество миллиардов операций с плавающей запятой в секунду. Эта метрика показывает скорость выполнения операций с плавающей запятой в нейронной сети и полезна для оценки производительности и эффективности работы модели.   

Я столкнулась с ситуацией, что обучение происходит медленно, а надо проделать тысячи итераций. Я меняла предварительно обученные веса; изменяла файл `Makefile` так, чтобы при обучении использовались `GPU`(графический процессор) и `CuDNN` (библиотека с поддержкой GPU примитивов для нейросетей. Она обеспечивает производительность, простоту в использовании и низкую нагрузку на память); и внесла изменения в файл с конфигурацией нейронной сети:  
```
batch=64  
subdivisions=16
max_batches=6000
steps=4800,5400
width=416
height=416
classes=1
filters=18
```
Благодаря проделанным действиям, обучение прошло быстро и не потеряло точность.  
Результаты обучения:  
![image](https://github.com/user-attachments/assets/07cea41f-2586-4aae-86a9-19934b25af69)  
