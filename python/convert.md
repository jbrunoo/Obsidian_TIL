
[github : pytorch - onnx - tflite](https://github.com/sithu31296/PyTorch-ONNX-TFLite)

https://da2so.tistory.com/56



input 값 1,1,1,1로 나오는 문제
```python
pip install tf-nightly
```

```python
# Before conversion, fix the model input size  
model = tf.saved_model.load("saved_model")  
model.signatures[tf.saved_model.DEFAULT_SERVING_SIGNATURE_DEF_KEY].inputs[0].set_shape([1, 300, 300, 3])  
tf.saved_model.save(model, "saved_model_updated", signatures=model.signatures[tf.saved_model.DEFAULT_SERVING_SIGNATURE_DEF_KEY])  
# Convert  
converter = tf.lite.TFLiteConverter.from_saved_model(saved_model_dir='saved_model_updated', signature_keys=['serving_default'])  
converter.optimizations = [tf.lite.Optimize.DEFAULT]  
converter.target_spec.supported_ops = [tf.lite.OpsSet.TFLITE_BUILTINS, tf.lite.OpsSet.SELECT_TF_OPS]  
tflite_model = converter.convert()  
  
## TFLite Interpreter to check input shape  
interpreter = tf.lite.Interpreter(model_content=tflite_model)  
interpreter.allocate_tensors()  
  
# Get input and output tensors.  
input_details = interpreter.get_input_details()  
output_details = interpreter.get_output_details()  
  
# Test the model on random input data.  
input_shape = input_details[0]['shape']  
print(input_shape)
```

tensorflow keras 버전 충돌로 보이는 거 해결하다가 그냥 여러 종속성끼리 충돌나버림 (포기)

근데 [여기서](https://github.com/tensorflow/tensorflow/issues/42157) 는 tflite가 동적 입력을 받고 1,1,1,3로 나온다고 적혀있음.? 내 코드에는 1,1,1,1로 나옴 입력 넣어봐야 알듯.

```
java.lang.IllegalStateException: Internal error: Unexpected failure when preparing tensor allocations: Regular TensorFlow ops are not supported by this interpreter. Make sure you apply/link the Flex delegate before inference.
                                                                                                    Node number 2 (FlexConv2D) failed to prepare.
```
flex delegate 를 convert 시 추가하라고 하긴 함

