# tensorRT

nvcr.io/nvidia/tensorrt:20.11-py3 도커 이미지 환경에서 구현하였습니다.


# onnx를 trt로의 변환

trtexec --onnx=ocr.onnx --minShapes=input:1x1x85x150 --optShapes=input:100x1x85x150 --maxShapes=input:200x1x85x150 --shapes=input:10x1x85x150 --saveEngine=ocr.engine --fp16 --workspace=256 ~~~
