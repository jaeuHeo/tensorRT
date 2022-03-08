# tensorRT

nvcr.io/nvidia/tensorrt:20.11-py3 도커 이미지 환경에서 구현하였습니다.


# onnx를 trt로의 변환

trtexec --onnx=ocr.onnx --minShapes=input:1x1x85x150 --optShapes=input:100x1x85x150 --maxShapes=input:200x1x85x150 --shapes=input:10x1x85x150 --saveEngine=ocr.engine --fp16 --workspace=256 ~~~

# terminate called after throwing an instance of 'std::out_of_range what(): Attribute not found: axes 에러 발생시 대처방법
현재 지원되지 않는 Unsqueeze의 opset 13 버전을 사용. 모델을 더 낮은 opset(즉, opset 11)으로 내보내야한다.
https://github.com/onnx/onnx-tensorrt/issues/670
