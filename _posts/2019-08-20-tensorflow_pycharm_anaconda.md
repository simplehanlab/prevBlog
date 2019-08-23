---
title: "[Tensorflow] Pycharm 설치 및 Anaconda 연동"
comments: true
categories:
  - tensorflow
tags:
  - python
  - tensorflow
  - deeplearning
date: "2019-08-20 17:20"
---

### 1. Pycharm  다운로드 및 설치 (community 버전)

[Pycharm 다운로드](https://www.jetbrains.com/pycharm/download){:target="_blank"} 

### 2. Pycharm - Anaconda 가상환경과 연동

1. 프로젝트 생성

   ![img](\assets\images\pycharm\pycharm_1.jpg)

   create new project 선택

2. 프로젝트 저장위치 지정 후 ▼ Project interpreter : Python 3.6 부분 선택

3. Existing interpreter 부분 경로 우측 ... 버튼 선택

   ![img](\assets\images\pycharm\pycharm_2.jpg)

4. Add Python Interpreter 창 내에 Conda Enviroment 탭 선택 

5. 우측 Conda executable 우측 폴더 아이콘 선택 후 \[설치한 Anaconda 경로￦envs￦생성한 가상환경￦python.exe \] 선택 후 OK 버튼 선택

6. ![img](\assets\images\pycharm\pycharm_3.jpg)

7. Existing interpreter 에 새로 지정한 경로가 입력되어 있는지 확인 후 create 

   ![img](\assets\images\pycharm\pycharm_3_2.jpg)

### 3. 가상환경과 연결 되어 있는지 확인

가상환경을 생성할 때 tensorflow 패키지를 같이 생성하였었다.  tensorflow 가 잘 생성 되었고, 제대로 동작 하는지 확인을 해보겠다.

1. Pyhon file 생성

   ![img](\assets\images\pycharm\pycharm_4.jpg)
   
2. 다음 코드 입력

   ```python
   import os
   import tensorflow as tf
   
   os.environ['TF_CPP_MIN_LOG_LEVEL'] = '3' 
   
   hello = tf.constant("hello")
   
   sess = tf.Session()
   print('version : ', tf.__version__)
   print(sess.run(hello))
   ```

   ```python
   os.environ['TF_CPP_MIN_LOG_LEVEL'] = '3' 
   ```
   
   이 부분은 <span style='color:red'>Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX AVX2</span>
   
   경고 문구를 없애기 위한 부분이다.
   
   위 코드를 실행해 보면 결과는 다음과 같다.
   
   ![img](\assets\images\pycharm\pycharm_5.jpg)
   
   tensorflow 버전과 hello 라는 문구가 찍혀 있는것을 볼 수 있다.

이것으로 Tensorflow를 공부하기 전에 필요한 환경과 Tool 을 셋팅 하였다. 앞으로 포스팅에는 이 환경을 이용하여 Tensorflow를 이용한 Deeplearning 에 대하여 다루도록 하겠다.