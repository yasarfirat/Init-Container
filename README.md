# Init-Container
This init container create a file in a volume, After this process the main container check this file exist. 
If file exist the main container goes sleep.

./
apiVersion: v1
kind: Pod
metadata:
  name: init-container
  labels:
   app: init-test
spec:
  containers:
  - name: main-container
    image: nginx
    command: ['sh', '-c', 'if [ -f /workdir/initfile.txt ]; then sleep 9999; else exit; fi']
    volumeMounts:
    - name: workdir
      mountPath: /workdir
  initContainers:
  - name: init-container
    image: busybox
    command: ['sh', '-c', 'mkdir /workdir; echo > /workdir/initfile.txt']
    volumeMounts:
    - name: workdir
      mountPath: /workdir
  volumes:
  - name: workdir
    emptyDir: {}
    
    ../
