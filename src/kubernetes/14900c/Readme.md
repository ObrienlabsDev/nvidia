see initial testing in https://github.com/ObrienlabsDev/blog/issues/139

```
before

ubuntu@14900c:~/wse_github/operator$ kubectl get node 14900c -o jsonpath='{.metadata.labels}'
{"beta.kubernetes.io/arch":"amd64","beta.kubernetes.io/instance-type":"rke2","beta.kubernetes.io/os":"linux","kubernetes.io/arch":"amd64","kubernetes.io/hostname":"14900c","kubernetes.io/os":"linux","node-role.kubernetes.io/control-plane":"true","node-role.kubernetes.io/etcd":"true","node.kubernetes.io/instance-type":"rke2"} 

apply
ubuntu@14900c:~/wse_github/operator$ kubectl apply -f gpu-operator.yaml 
helmchart.helm.cattle.io/gpu-operator created

after

ubuntu@14900c:~/wse_github/operator$ kubectl get nodes
NAME     STATUS   ROLES                AGE    VERSION
14900c   Ready    control-plane,etcd   132d   v1.35.6+rke2r1

ubuntu@14900c:~/wse_github/operator$ kubectl get node 14900c -o jsonpath='{.metadata.labels}' |  grep "nvidia.com"

{"beta.kubernetes.io/arch":"amd64","beta.kubernetes.io/instance-type":"rke2","beta.kubernetes.io/os":"linux","feature.node.kubernetes.io/cpu-cpuid.ADX":"true","feature.node.kubernetes.io/cpu-cpuid.AESNI":"true","feature.node.kubernetes.io/cpu-cpuid.AVX":"true","feature.node.kubernetes.io/cpu-cpuid.AVX2":"true","feature.node.kubernetes.io/cpu-cpuid.AVXVNNI":"true","feature.node.kubernetes.io/cpu-cpuid.BHI_CTRL":"true","feature.node.kubernetes.io/cpu-cpuid.CETIBT":"true","feature.node.kubernetes.io/cpu-cpuid.CETSS":"true","feature.node.kubernetes.io/cpu-cpuid.CMPXCHG8":"true","feature.node.kubernetes.io/cpu-cpuid.FLUSH_L1D":"true","feature.node.kubernetes.io/cpu-cpuid.FMA3":"true","feature.node.kubernetes.io/cpu-cpuid.FSRM":"true","feature.node.kubernetes.io/cpu-cpuid.FXSR":"true","feature.node.kubernetes.io/cpu-cpuid.FXSROPT":"true","feature.node.kubernetes.io/cpu-cpuid.GFNI":"true","feature.node.kubernetes.io/cpu-cpuid.HRESET":"true","feature.node.kubernetes.io/cpu-cpuid.HYBRID_CPU":"true","feature.node.kubernetes.io/cpu-cpuid.IA32_ARCH_CAP":"true","feature.node.kubernetes.io/cpu-cpuid.IA32_CORE_CAP":"true","feature.node.kubernetes.io/cpu-cpuid.IBPB":"true","feature.node.kubernetes.io/cpu-cpuid.IDPRED_CTRL":"true","feature.node.kubernetes.io/cpu-cpuid.KEYLOCKER":"true","feature.node.kubernetes.io/cpu-cpuid.LAHF":"true","feature.node.kubernetes.io/cpu-cpuid.MD_CLEAR":"true","feature.node.kubernetes.io/cpu-cpuid.MOVBE":"true","feature.node.kubernetes.io/cpu-cpuid.MOVDIR64B":"true","feature.node.kubernetes.io/cpu-cpuid.MOVDIRI":"true","feature.node.kubernetes.io/cpu-cpuid.OSXSAVE":"true","feature.node.kubernetes.io/cpu-cpuid.PCONFIG":"true","feature.node.kubernetes.io/cpu-cpuid.PMU_FIXEDCOUNTER_CYCLES":"true","feature.node.kubernetes.io/cpu-cpuid.PMU_FIXEDCOUNTER_INSTRUCTIONS":"true","feature.node.kubernetes.io/cpu-cpuid.PMU_FIXEDCOUNTER_REFCYCLES":"true","feature.node.kubernetes.io/cpu-cpuid.PSFD":"true","feature.node.kubernetes.io/cpu-cpuid.RRSBA_CTRL":"true","feature.node.kubernetes.io/cpu-cpuid.SERIALIZE":"true","feature.node.kubernetes.io/cpu-cpuid.SHA":"true","feature.node.kubernetes.io/cpu-cpuid.SPEC_CTRL_SSBD":"true","feature.node.kubernetes.io/cpu-cpuid.STIBP":"true","feature.node.kubernetes.io/cpu-cpuid.STOSB_SHORT":"true","feature.node.kubernetes.io/cpu-cpuid.SYSCALL":"true","feature.node.kubernetes.io/cpu-cpuid.SYSEE":"true","feature.node.kubernetes.io/cpu-cpuid.TME":"true","feature.node.kubernetes.io/cpu-cpuid.VAES":"true","feature.node.kubernetes.io/cpu-cpuid.VMX":"true","feature.node.kubernetes.io/cpu-cpuid.VPCLMULQDQ":"true","feature.node.kubernetes.io/cpu-cpuid.WAITPKG":"true","feature.node.kubernetes.io/cpu-cpuid.X86_64_V1":"true","feature.node.kubernetes.io/cpu-cpuid.X86_64_V2":"true","feature.node.kubernetes.io/cpu-cpuid.X86_64_V3":"true","feature.node.kubernetes.io/cpu-cpuid.X87":"true","feature.node.kubernetes.io/cpu-cpuid.XGETBV1":"true","feature.node.kubernetes.io/cpu-cpuid.XSAVE":"true","feature.node.kubernetes.io/cpu-cpuid.XSAVEC":"true","feature.node.kubernetes.io/cpu-cpuid.XSAVEOPT":"true","feature.node.kubernetes.io/cpu-cpuid.XSAVES":"true","feature.node.kubernetes.io/cpu-cstate.enabled":"true","feature.node.kubernetes.io/cpu-hardware_multithreading":"false","feature.node.kubernetes.io/cpu-model.family":"6","feature.node.kubernetes.io/cpu-model.hypervisor":"none","feature.node.kubernetes.io/cpu-model.id":"183","feature.node.kubernetes.io/cpu-model.vendor_id":"Intel","feature.node.kubernetes.io/cpu-pstate.scaling_governor":"powersave","feature.node.kubernetes.io/cpu-pstate.status":"active","feature.node.kubernetes.io/cpu-pstate.turbo":"true","feature.node.kubernetes.io/kernel-config.NO_HZ":"true","feature.node.kubernetes.io/kernel-config.NO_HZ_FULL":"true","feature.node.kubernetes.io/kernel-version.full":"6.17.0-41-generic","feature.node.kubernetes.io/kernel-version.major":"6","feature.node.kubernetes.io/kernel-version.minor":"17","feature.node.kubernetes.io/kernel-version.revision":"0","feature.node.kubernetes.io/memory-swap":"true","feature.node.kubernetes.io/pci-10de.present":"true","feature.node.kubernetes.io/pci-1d6a.present":"true","feature.node.kubernetes.io/pci-8086.present":"true","feature.node.kubernetes.io/storage-nonrotationaldisk":"true","feature.node.kubernetes.io/system-os_release.ID":"ubuntu","feature.node.kubernetes.io/system-os_release.VERSION_ID":"25.10","feature.node.kubernetes.io/system-os_release.VERSION_ID.major":"25","feature.node.kubernetes.io/system-os_release.VERSION_ID.minor":"10","feature.node.kubernetes.io/usb-ef_0b05_1a53.present":"true","feature.node.kubernetes.io/usb-ff_0b05_18f3.present":"true","kubernetes.io/arch":"amd64","kubernetes.io/hostname":"14900c","kubernetes.io/os":"linux","node-role.kubernetes.io/control-plane":"true","node-role.kubernetes.io/etcd":"true","node.kubernetes.io/instance-type":"rke2","nvidia.com/cuda.driver-version.full":"595.71.05","nvidia.com/cuda.driver-version.major":"595","nvidia.com/cuda.driver-version.minor":"71","nvidia.com/cuda.driver-version.revision":"05","nvidia.com/cuda.driver.major":"595","nvidia.com/cuda.driver.minor":"71","nvidia.com/cuda.driver.rev":"05","nvidia.com/cuda.runtime-version.full":"13.2","nvidia.com/cuda.runtime-version.major":"13","nvidia.com/cuda.runtime-version.minor":"2","nvidia.com/cuda.runtime.major":"13","nvidia.com/cuda.runtime.minor":"2","nvidia.com/gfd.timestamp":"1795400339","nvidia.com/gpu.compute.major":"8","nvidia.com/gpu.compute.minor":"6","nvidia.com/gpu.count":"1","nvidia.com/gpu.deploy.client":"true","nvidia.com/gpu.deploy.container-toolkit":"true","nvidia.com/gpu.deploy.dcgm":"true","nvidia.com/gpu.deploy.dcgm-exporter":"true","nvidia.com/gpu.deploy.device-plugin":"true","nvidia.com/gpu.deploy.driver":"pre-installed","nvidia.com/gpu.deploy.gpu-feature-discovery":"true","nvidia.com/gpu.deploy.node-status-exporter":"true","nvidia.com/gpu.deploy.operator-validator":"true","nvidia.com/gpu.family":"ampere","nvidia.com/gpu.machine":"System-Product-Name","nvidia.com/gpu.memory":"49140","nvidia.com/gpu.mode":"graphics","nvidia.com/gpu.present":"true","nvidia.com/gpu.product":"NVIDIA-RTX-A6000","nvidia.com/gpu.replicas":"1","nvidia.com/gpu.sharing-strategy":"none","nvidia.com/mig.capable":"false","nvidia.com/mig.strategy":"single","nvidia.com/mps.capable":"false","nvidia.com/vgpu.present":"false"}


for example

nvidia.com/cuda.driver.major":"595",

```


testing
https://github.com/NVIDIA/gpu-operator/blob/main/tests/gpu-pod.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: gpu-operator-test
spec:
  restartPolicy: OnFailure
  containers:
  - name: cuda-vector-add
    image: "nvcr.io/nvidia/k8s/cuda-sample:vectoradd-cuda12.5.0-ubuntu22.04"
    resources:
      limits:
        nvidia.com/gpu: 1


ubuntu@14900c:~/wse_github/obrienlabsdev/nvidia/src/kubernetes/14900c$ vi gpu-operator-test-pod.yaml
ubuntu@14900c:~/wse_github/obrienlabsdev/nvidia/src/kubernetes/14900c$ kubectl apply -f gpu-operator-test-pod.yaml 
pod/gpu-operator-test created
ubuntu@14900c:~/wse_github/obrienlabsdev/nvidia/src/kubernetes/14900c$ kubectl logs gpu-operator-test 
[Vector addition of 50000 elements]
Copy input data from the host memory to the CUDA device
CUDA kernel launch with 196 blocks of 256 threads
Copy output data from the CUDA device to the host memory
Test PASSED
Done
```

