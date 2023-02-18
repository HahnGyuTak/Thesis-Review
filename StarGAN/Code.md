# Analysis Code

## Model.py

* ResidualBlock 클래스  --  nn.Module 상속
  * Convolution2D
  * InstanceNorm2d (인스턴스 정규화)
  * ReLU (활성화 렐루 함수)
  * Convolution2D
  * InstanceNorm2d

* Generator 클래스(생성자) --  nn.Module 상속
  * init
    * Down Sampling Layers (~Pooling)
    * Bottleneck layers 차원 축소 ResidualBlock 사용)
    * Up-sampling layers ( ~~ Unpooling) -- 원본이미지로 다시 복구
  
* Generator 클래스(생성자) --  nn.Module 상속

## Data_Loader.py

* CelebA 클래스
  * init
        mode 가 train 혹은 test에 따라 호출하는 데이터셋이 달라짐
  * preprocess
        경로를 읽어서 데이터 취합 후 2000개 이하이면 test데이터, 아니면 train데이터에 추가

* *get_loader* (image_dir, attr_path, selected_attrs, crop_size=178, image_size=128, batch_size=16, dataset='CelebA', mode='train', num_workers=1):
  * train 모드일 경우, torchvision.transforms의 RandomHorizontalFlip으로 좌우반전
  * 이미지 중앙 추출
  * 크기 조정
  * 배열구조 변경(HWC->CHW)
  * 정규화
  * Compose를 통해 위 변환단계를 하나로 묶음

## Solver.py

* Solver클래스
  * build_model
    * self에 Generator와 Discriminator 연결
    * 각 G와 D에 Adam Optimizer 연결
  * print_network
    * 모델 파라미터 출력
  * restore_model
    * 경로에 저장된 모델을 불러옴
  * build_tensorboard
    * 기록과 저장을 위한 logger 호출
  * update_lr
    * learning rate 업데이트
  * reset_grad
    * 역전파를 하기전에 otimizer 초기화 (zero_grad)
  * denorm
    * -1 ~ 1 사이의 값을 0 ~ 1 사이로 변환
  * gradient_penalty (?)
    * 기울기 패널티 계산
  * label2onehot 
    * label 인덱스를 one-hot 인코딩 형식으로 변환
  * create_labels (?)
    * Generate target domain labels for debugging and testing
    * 레이블 생성
  * classification_loss
    * Compute binary or softmax cross entropy loss
    * CelebA 데이터일 경우, binary cross entropy Loss 계산
    * RaFD 데이터일 경우, 그냥 cross entropy Loss 계산
  * *train*
    * 싱글데이터로 StarGAN 학습
      1. Preprocess input data
         * 랜덤으로 target domain label 생성
      2. Train the discriminator
         * 실제 이미지 loss 계산
         * fake 이미지 loss 계산
         * Gradient panelty 의 loss 계산
         * 총 loss 계산 후 역전파(backward)
      3. Train the generator
         * 생성자(Generator)와 실제 이미지로 fake이미지 생성
         * 판별자(Discriminator)에 fake이미지 입력 후 판별
         * loss계산
         * 역전파 실행
      4. Miscellaneous
         * 학습 정보 출력
         * 이미지 저장
         * 모델 체크포인트 저장
         * learning rate 업데이트

  * *train_multi*
    * multi데이터로 StarGAN 학습
      1. 
