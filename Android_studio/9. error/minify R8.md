
> Task :app:minifyReleaseWithR8 FAILED

ERROR: Missing classes detected while running R8. Please add the missing classes or apply additional keep rules that are generated in /Users/jbrunoo/digitInk/app/build/outputs/mapping/release/missing_rules.txt.

ERROR: R8: Missing class org.tensorflow.lite.gpu.GpuDelegateFactory$Options$GpuBackend (referenced from: void org.tensorflow.lite.gpu.GpuDelegate.<init>(org.tensorflow.lite.gpu.GpuDelegateFactory$Options))

Missing class org.tensorflow.lite.gpu.GpuDelegateFactory$Options (referenced from: void org.tensorflow.lite.gpu.GpuDelegate.<init>() and 1 other context)

  
implementation("org.tensorflow:tensorflow-lite-gpu-api:2.17.0") 추가하여 해결