1. 기존 tensorRT 6.x 버전은 지원문제때문에 사용하지 않기로 하고 다시 7.x 버전이상의 도커를 다시 띄움

2. 1번으로 torch → onnx, onnx → trt 변환 성공했음. 이전 tensorRT 6.x 버전쓸때는 아마 레이어 지원의 한계로 onnx → trt 가 안된것으로 추정

3. 7.x 버전의 tensorRT 도커 위에서 CRAFT 모델의 torch to onnx, onnx to trt 를 진행

4. 하지만, 원인 모를 문제가 생겼으나 단순 opencv 문제였음

5. torch to onnx, onnx to trt 방식으로 craft 모델의 성능과 기존 모델의 비교를 통해 결괏값 보기
    score_text의 결과가 네개로 나옴... -> 이건 아직 이유를 모름
    모델 로드 시간:
    기존 craft 모델: 9.8초
    trt craft 모델: 14.2초
    모델 추론 시간
    기존 craft 모델: 0.5초
    trt craft 모델: 4.8초
    모델 로드 메모리
    기존 craft 모델: 81mb
    trt craft 모델: 0mb
    모델 추론 메모리
    기존 craft 모델: 41mb
    trt craft 모델: 0mb
    
6. trt 모델은 왜 시간이 더 오래걸리고 쿠다 메모리를 0으로 잡는지...?
    nvidia-smi 에서는 모델 로드 : 약 400MB, 추론: 1000MB 정도 잡음
    기존 모델은 로드와 추론 각각 1400MB, 600MB 정도 할당됨
    
7. 기존 모델은 애초에 모델 로드할 때 여유메모리를 크게 잡아서 image.cuda() 할때 시간이 0.1초대이지만 trt 모델은 모델 로드할 때 적은 여유메모리를 잡고 image.cuda() 할때 크게 잡아서 올리기 때문에 시간이 5초 이상 걸리는 것으로 추정

8. 