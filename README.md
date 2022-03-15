# tensorRT

nvcr.io/nvidia/tensorrt:20.11-py3 도커 이미지 환경에서 구현하였습니다.


# onnx를 trt로의 변환

trtexec --onnx=ocr.onnx --minShapes=input:1x1x85x150 --optShapes=input:100x1x85x150 --maxShapes=input:200x1x85x150 --shapes=input:10x1x85x150 --saveEngine=ocr.engine --fp16 --workspace=256 ~~~

# terminate called after throwing an instance of 'std::out_of_range what(): Attribute not found: axes 에러 발생시 대처방법
현재 지원되지 않는 Unsqueeze의 opset 13 버전을 사용. 모델을 더 낮은 opset(즉, opset 11)으로 내보내야한다.
https://github.com/onnx/onnx-tensorrt/issues/670

# tensorrt8버전으로 이전하기 위해 tensorrt:22.01-py3 도커 이미지를 띄웠습니다.

# pycuda 에러시 (2020.1 버전 기준)
#### $ wget https://files.pythonhosted.org/packages/5a/56/4682a5118a234d15aa1c8768a528aac4858c7b04d2674e18d586d3dfda04/pycuda-2020.1.tar.gz
#### $ tar xzf pycuda-2020.1.tar.gz
#### $ cd pycuda-2020.1
#### $ python configure.py --cuda-root=/usr/local/cuda
#### $ make install

# tensorRT error 해결

### trt engine load & inference 문제 

tensorRT 21.09 컨테이너를 띄운다

컨테이너 내부에 오픈 소스 구성 요소를 설치한다

오픈 소스 구성 요소 설치

제공된 플러그인, Caffe 파서 및 ONNX 파서 라이브러리를 공식 TensorRT 오픈 소스 리포지토리의 21.09 태그를 기반으로 하는 오픈 소스 라이브러리로 복제, 빌드 및 교체하는 스크립트가 추가되었습니다.

컨테이너 내부에 오픈 소스 구성 요소를 설치하려면 다음 스크립트를 실행합니다.

/opt/tensorrt/install_opensource.sh

자세한 내용은 GitHub: TensorRT 21.09 를 참조하세요 .

torch 버전은 현재 cuda 버전과 동일 및 가까운 버전을 install 한다

### 전체적으로 작동하는데에는 문제는 없으나 다음과 같은 에러가 발생 

[TensorRT] ERROR: 1: [convolutionRunner.cpp::checkCaskExecError<false>::440] Error Code 1: Cask (Cask Convolution execution)
  
[TensorRT] ERROR: 1: [apiCheck.cpp::apiCatchCudaError::17] Error Code 1: Cuda Runtime (invalid resource handle)
  
위 에러는 아마 캐싱 문제인 것으로 판단됨... apiCatchCudaError...

해결해씀..!!!!

단지 uwsgi 에서 디장고 thread를 1로 해주면 됨...프로세스도 상관없이...

그리고 추가로 1 인퍼런스가 끝나면 allocatation된 값 지워주는 것 추가햇음
