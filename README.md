# ☁️ Modify & Re-deploy Java OCI Function via OCI Cloud Shell

Summary
```
cd ~/functions/pdf-unlock-func
nano src/main/java/com/pos28/fn/Main.java    # modify
fn build
docker login mytenancy.ocir.io               # one-time login
docker tag c0ffee12345 mytenancy.ocir.io/functions/pdf-unlock-func:v2
docker push mytenancy.ocir.io/functions/pdf-unlock-func:v2
oci fn function update --function-id <fn-ocid> --image mytenancy.ocir.io/functions/pdf-unlock-func:v2
fn invoke pdf-unlock-app pdf-unlock-func
