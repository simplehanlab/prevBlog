---
title: "[Tensorflow] Anaconda(아나콘다) 설치하기"
comments: true
categories:
  - tensorflow
tags:
  - python
  - tensorflow
  - deeplearning
date: "2019-08-20 16:00"
---

### 아나콘다

- 파이썬을 가상환경으로 사용할 수 있도록 지원하는 프로젝트중 하나이다.
- 파이썬 및 주로 사용되는 1400 여개의 패키지와 데이터 과학 패키지들이 포함되어 있어 파이썬 설치 후 여러 패키지들을 설치해야 하는 번거로움을 해결해 준다.
- 윈도우즈에서 쉽게 가상환경을 만들고 버전 관리를 할 수 있도록 도와준다.
- 설치 시간이 좀 오래 걸린다는 단점이 있다.

### 윈도우용 아나콘다 설치 

1. [아나콘다 다운로드](https://www.anaconda.com/download/){:target="_blank"} 페이지에서 자신에게 맞는 운영체제의 파일을 다운로드한다.

   ![img](\assets\images\install_anaconda\install_anaconda_1.jpg)

   (Windows용 텐서플로 바이너리 패키지는 현재 파이썬 3.5, 3.6, 3.7버전을 지원한다.)

2. 파이썬 설치프로그램 실행

   ![img](\assets\images\install_anaconda\install_anaconda_2.jpg)

   Next > 를 선택한다. 

3. License Agreement

   ![img](\assets\images\install_anaconda\install_anaconda_3.jpg)

   I Agree 선택

4. Select Installation Type

   ![img](\assets\images\install_anaconda\install_anaconda_4.jpg)

   just me 선택

   (All user 를 선택할시 C 드라이브 프로그램에 설치가 된다. 로컬PC 에서 테스트 용을 사용할 시에는 just me 를선택할 것을 권장한다.)

5. Choose Install Location

   ![img](\assets\images\install_anaconda\install_anaconda_5.jpg)

   로컬 PC 사용자 폴더에 설치가 되어진다

   ![img](\assets\images\install_anaconda\install_anaconda_5_1.jpg)
   
   (사용자 계정이 한글일 경우 다른 경로를 입력 하라는 경고창이 뜨게 된다. 한글이 포함되지 않은 경로를 재설정 하여 설치를 계속 진행한다.)
   
6. Advanced Options

   ![img](\assets\images\install_anaconda\install_anaconda_6.jpg)

   1. Add Anaconda to my PATH enviroment variable
      - 아나콘다를 PATH 환경 변수에 추가할지 여부를 선택하는 부분
      - 환경 변수에 추가 하면 윈도우 CMD 창에서 conda 명령어를 사용할 수 있으면 선택하지 않을 경우 Anaconda prompt 또는 Anaconda navigator 를 실행하여야 conda 명령어를 사용할 수 있다.
      - 다른 Python 은 설치 하지 않고 Anaconda 를 주력으로 사용할 경우에만 체크할 것을 권유한다. 
   2. Regster Anaconda as my default Python 3.7
      - 아나콘다를 기본 파이썬으로 등록 할지 여부를 선택한다.
      - 선택 시 개발 도구나 에디터에서 아나콘다를 파이썬으로 인식하게 된다.

7. 설치완료 - 1 (VSCode 설치여부)

   ![img](\assets\images\install_anaconda\install_anaconda_7.jpg)

   이미 설치 되어 있거나 VSCode를 에디터로 사용하지 않을 경우는 Skip을 선택한다.

8. 설치 완료

   ![img](\assets\images\install_anaconda\install_anaconda_8.jpg)

9. 시작 메뉴 확인

   ![img](\assets\images\install_anaconda\install_anaconda_9.jpg)

### 가상환경 설정

- 꼭 python 3.7 을 사용 하지 않고 다른 버전의 python 을 사용하기 위한 가상 환경을 생성 할 수 있다.
- 나중에 Pycharm 과 연동하여 다양한 환경에서의 개발을 진행 할 수 있다.

1. Anaconda prompt 실행

   Anaconda Navigator를 이용하여 GUI 환경에서 설정이 가능하지만 이번에는 prompt 명령어를 통해서 가상환경을 생성해 본다.

   설치한 아나콘다가 맞는지 버전을 확인해 본다

   ```bash
   $ conda --version
   ```

   ![img](\assets\images\install_anaconda\set_virtual_envs_1.jpg)

2. 가상환경 생성

   가상환경을 생성한다. 이 때 필요한 패키지들을 나열해서 생성시 한번에 설치할 수 있다.

   ```bash
   $ conda create --name [MY_ENV_NAME] [package1 package2 ....]
   ```

   이번 시간엔  Tensorflow 를 사용하기 위해서 파이썬 3.6 버전과 tensorflow 와 keras 를 명시하여 설치해 보겠다.

   ```bash
   $ conda create --name TEST_ENV python=3.6 tensorflow keras
   ```

   생성 진행 화면

   ```bash
   (base) C:\Users\이한얼>conda create --name TEST_ENV python=3.6 tensorflow keras
   Solving environment: done
   
   
   ==> WARNING: A newer version of conda exists. <==
     current version: 4.5.4
     latest version: 4.7.11
   
   Please update conda by running
   
       $ conda update -n base conda
   
   
   
   ## Package Plan ##
   
     environment location: D:\Anaconda3\envs\TEST_ENV
   
     added / updated specs:
       - keras
       - python=3.6
       - tensorflow
   
   
   The following packages will be downloaded:
   
       package                    |            build
       ---------------------------|-----------------
       _tflow_select-2.3.0        |              mkl           3 KB
       numpy-1.16.4               |   py36h19fb1c0_0          49 KB
       numpy-base-1.16.4          |   py36hc3f5095_0         4.1 MB
       wrapt-1.11.2               |   py36he774522_0          48 KB
       pyyaml-5.1.2               |   py36he774522_0         163 KB
       intel-openmp-2019.4        |              245         1.7 MB
       ca-certificates-2019.5.15  |                1         166 KB
       tensorflow-base-1.14.0     |mkl_py36ha978198_0        54.5 MB
       scipy-1.3.1                |   py36h29ff71c_0        14.4 MB
       python-3.6.9               |       h5500b2f_0        20.4 MB
       termcolor-1.1.0            |           py36_1           8 KB
       wheel-0.33.4               |           py36_0          57 KB
       astor-0.8.0                |           py36_0          45 KB
       absl-py-0.7.1              |           py36_0         158 KB
       mkl-service-2.0.2          |   py36he774522_0          63 KB
       h5py-2.9.0                 |   py36h5e291fa_0         969 KB
       six-1.12.0                 |           py36_0          22 KB
       sqlite-3.29.0              |       he774522_0         962 KB
       mkl_fft-1.0.14             |   py36h14836fe_0         137 KB
       mkl_random-1.0.2           |   py36h343c172_0         318 KB
       icc_rt-2019.0.0            |       h0cc432a_1         9.4 MB
       tensorflow-1.14.0          |mkl_py36hb88db5b_0           4 KB
       keras-2.2.4                |                0           5 KB
       mkl-2019.4                 |              245       157.5 MB
       protobuf-3.8.0             |   py36h33f27b4_0         582 KB
       zlib-1.2.11                |       h62dcd97_3         128 KB
       libmklml-2019.0.5          |                0        21.4 MB
       gast-0.2.2                 |           py36_0         138 KB
       setuptools-41.0.1          |           py36_0         663 KB
       pyreadline-2.1             |           py36_1         141 KB
       libprotobuf-3.8.0          |       h7bd577a_0         2.2 MB
       werkzeug-0.15.5            |             py_0         256 KB
       markdown-3.1.1             |           py36_0         132 KB
       pip-19.2.2                 |           py36_0         1.9 MB
       certifi-2019.6.16          |           py36_1         156 KB
       keras-applications-1.0.8   |             py_0          33 KB
       tensorflow-estimator-1.14.0|             py_0         291 KB
       tensorboard-1.14.0         |   py36he3c9ec2_0         3.3 MB
       openssl-1.1.1c             |       he774522_1         5.7 MB
       keras-preprocessing-1.1.0  |             py_1          36 KB
       hdf5-1.10.4                |       h7ebc959_0        19.2 MB
       keras-base-2.2.4           |           py36_0         458 KB
       grpcio-1.16.1              |   py36h351948d_1         931 KB
       ------------------------------------------------------------
                                              Total:       322.9 MB
   
   The following NEW packages will be INSTALLED:
   
       _tflow_select:        2.3.0-mkl
       absl-py:              0.7.1-py36_0
       astor:                0.8.0-py36_0
       blas:                 1.0-mkl
       ca-certificates:      2019.5.15-1
       certifi:              2019.6.16-py36_1
       gast:                 0.2.2-py36_0
       grpcio:               1.16.1-py36h351948d_1
       h5py:                 2.9.0-py36h5e291fa_0
       hdf5:                 1.10.4-h7ebc959_0
       icc_rt:               2019.0.0-h0cc432a_1
       intel-openmp:         2019.4-245
       keras:                2.2.4-0
       keras-applications:   1.0.8-py_0
       keras-base:           2.2.4-py36_0
       keras-preprocessing:  1.1.0-py_1
       libmklml:             2019.0.5-0
       libprotobuf:          3.8.0-h7bd577a_0
       markdown:             3.1.1-py36_0
       mkl:                  2019.4-245
       mkl-service:          2.0.2-py36he774522_0
       mkl_fft:              1.0.14-py36h14836fe_0
       mkl_random:           1.0.2-py36h343c172_0
       numpy:                1.16.4-py36h19fb1c0_0
       numpy-base:           1.16.4-py36hc3f5095_0
       openssl:              1.1.1c-he774522_1
       pip:                  19.2.2-py36_0
       protobuf:             3.8.0-py36h33f27b4_0
       pyreadline:           2.1-py36_1
       python:               3.6.9-h5500b2f_0
       pyyaml:               5.1.2-py36he774522_0
       scipy:                1.3.1-py36h29ff71c_0
       setuptools:           41.0.1-py36_0
       six:                  1.12.0-py36_0
       sqlite:               3.29.0-he774522_0
       tensorboard:          1.14.0-py36he3c9ec2_0
       tensorflow:           1.14.0-mkl_py36hb88db5b_0
       tensorflow-base:      1.14.0-mkl_py36ha978198_0
       tensorflow-estimator: 1.14.0-py_0
       termcolor:            1.1.0-py36_1
       vc:                   14.1-h0510ff6_4
       vs2015_runtime:       14.15.26706-h3a45250_4
       werkzeug:             0.15.5-py_0
       wheel:                0.33.4-py36_0
       wincertstore:         0.2-py36h7fe50ca_0
       wrapt:                1.11.2-py36he774522_0
       yaml:                 0.1.7-hc54c509_2
       zlib:                 1.2.11-h62dcd97_3
   
   Proceed ([y]/n)? y
   
   
   Downloading and Extracting Packages
   _tflow_select-2.3.0  |    3 KB | ###################################### | 100%
   numpy-1.16.4         |   49 KB | ###################################### | 100%
   numpy-base-1.16.4    |  4.1 MB | ###################################### | 100%
   wrapt-1.11.2         |   48 KB | ###################################### | 100%
   pyyaml-5.1.2         |  163 KB | ###################################### | 100%
   intel-openmp-2019.4  |  1.7 MB | ###################################### | 100%
   ca-certificates-2019 |  166 KB | ###################################### | 100%
   tensorflow-base-1.14 | 54.5 MB | ###################################### | 100%
   scipy-1.3.1          | 14.4 MB | ###################################### | 100%
   python-3.6.9         | 20.4 MB | ###################################### | 100%
   termcolor-1.1.0      |    8 KB | ###################################### | 100%
   wheel-0.33.4         |   57 KB | ###################################### | 100%
   astor-0.8.0          |   45 KB | ###################################### | 100%
   absl-py-0.7.1        |  158 KB | ###################################### | 100%
   mkl-service-2.0.2    |   63 KB | ###################################### | 100%
   h5py-2.9.0           |  969 KB | ###################################### | 100%
   six-1.12.0           |   22 KB | ###################################### | 100%
   sqlite-3.29.0        |  962 KB | ###################################### | 100%
   mkl_fft-1.0.14       |  137 KB | ###################################### | 100%
   mkl_random-1.0.2     |  318 KB | ###################################### | 100%
   icc_rt-2019.0.0      |  9.4 MB | ###################################### | 100%
   tensorflow-1.14.0    |    4 KB | ###################################### | 100%
   keras-2.2.4          |    5 KB | ###################################### | 100%
   mkl-2019.4           | 157.5 MB | ##################################### | 100%
   protobuf-3.8.0       |  582 KB | ###################################### | 100%
   zlib-1.2.11          |  128 KB | ###################################### | 100%
   libmklml-2019.0.5    | 21.4 MB | ###################################### | 100%
   gast-0.2.2           |  138 KB | ###################################### | 100%
   setuptools-41.0.1    |  663 KB | ###################################### | 100%
   pyreadline-2.1       |  141 KB | ###################################### | 100%
   libprotobuf-3.8.0    |  2.2 MB | ###################################### | 100%
   werkzeug-0.15.5      |  256 KB | ###################################### | 100%
   markdown-3.1.1       |  132 KB | ###################################### | 100%
   pip-19.2.2           |  1.9 MB | ###################################### | 100%
   certifi-2019.6.16    |  156 KB | ###################################### | 100%
   keras-applications-1 |   33 KB | ###################################### | 100%
   tensorflow-estimator |  291 KB | ###################################### | 100%
   tensorboard-1.14.0   |  3.3 MB | ###################################### | 100%
   openssl-1.1.1c       |  5.7 MB | ###################################### | 100%
   keras-preprocessing- |   36 KB | ###################################### | 100%
   hdf5-1.10.4          | 19.2 MB | ###################################### | 100%
   keras-base-2.2.4     |  458 KB | ###################################### | 100%
   grpcio-1.16.1        |  931 KB | ###################################### | 100%
   Preparing transaction: done
   Verifying transaction: done
   Executing transaction: done
   #
   # To activate this environment, use
   #
   #     $ conda activate TEST_ENV
   #
   # To deactivate an active environment, use
   #
   #     $ conda deactivate
   
   ```

   가상 환경이 생성 되었다. 이제 생성한 가상 환경으로 접속을 진행해 본다.

   ※ 가상환경을 생성한 뒤에 pip을 통해 필요한 패키지들을 설치하는것도 가능하다.

3. 가상환경 접속

   ```bash
   $ conda activate [MY_ENVS_NAME]
   ```

   생성한 가상환경 이름으로 접속

   ```bash
   $ conda activate TEST_ENV
   ```

   ![img](\assets\images\install_anaconda\set_virtual_envs_2.jpg)

   - 설치한 python 버전 확인

     ```
     (TEST_ENV) C:\Users\이한얼>python --version
     ```

     ![img](\assets\images\install_anaconda\set_virtual_envs_3.jpg)

4. conda 기본 명령어들

   \# 가상환경 종료

   ```bash
   $ conda deactivate
   ```

   \#가상환경 목록 화인

   ```bash
   $ conda env list
   ```

   \#가상환경 삭제

   ```bash
   $ conda remove --name [MY_ENVS_NAME] --all
   ```

5. 가상환경 생성시 설치한 tensorflow 확인

   tensorflow  패키지가 정상적으로 설치 되었는지 버전 확인

   1. python 명령어로 python shell  진입

      ```bash
      $ python
      ```

   2. tensorflow import 후 버전확인

      ```bash
      $ import tensorflow as tf
      $ tf.__version__
      ```

   ![img](\assets\images\install_anaconda\set_virtual_envs_4.jpg)

Anaconda를 이용한 가상환경 생성 및 기본적으로 쓰이는 명령어에 대해서 알아 보았다. 다음 포스팅에는 Anaconda를 통해 생성한 가상환경을 Python IDE 중 하나인 Pycharm과 연동하여 Python을 이용하는 방법을 알아보겠다.