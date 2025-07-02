
## ✅ Prerequisites

- Function was created via fn init in OCI Cloud Shell
- You have access to OCI Console and Cloud Shell
- You know:
  - Your tenancy namespace (for OCIR)
  - Your OCI username
  - Your Auth Token (not OCI password)
- You know your Function's Application Name and optionally the Function OCID

---

## 🧭 Step 1: Open Cloud Shell and Navigate to Project

Launch Cloud Shell from the OCI Console (top-right icon), then:

bash
````
cd ~/functions/pdf-unlock-func
````
Confirm presence of:
```
func.yaml
src/main/java/com/fun/fn/Main.java
```

---

✏️ Step 2: Modify Java Code

Open the Java file in a terminal editor:
```
nano src/main/java/com/fun/fn/Main.java
```
Paste your modified code. 
Ctrl + O → Enter → Ctrl + X



---

🧱 Step 3: Build the Function
```
fn build
```
This compiles the code and builds a Docker image locally in Cloud Shell.


---

🔐 Step 4: Docker Login to Oracle Container Registry (OCIR)

First, get your tenancy namespace from:

Console → Object Storage → Namespace


Then run:
```
docker login <namespace>.ocir.io
```
Example:
```
docker login mytenancy.ocir.io
Username: mytenancy/Trojan.Tarun@gmail.com
Password: <your-auth-token>
```
You must use an OCI Auth Token, not your OCI password.


---

🏷️ Step 5: Tag and Push the Docker Image

Get the image ID:
```
docker images
```
Tag it for OCIR:
```
docker tag <image-id> mytenancy.ocir.io/functions/pdf-unlock-func:v2
```
Push to OCIR:
```
docker push mytenancy.ocir.io/functions/pdf-unlock-func:v2
```

---

🔁 Step 6: Update OCI Function to Use New Image

First, get the function OCID (if unknown):
```
oci fn function list --application-id <your-app-ocid>
```
Update it:
```
oci fn function update \
  --function-id ocid1.fnfunc.oc1..aaaaaaaaxyz \
  --image mytenancy.ocir.io/functions/pdf-unlock-func:v2
```

---

▶️ Step 7: Invoke and Test Function

fn invoke pdf-unlock-app pdf-unlock-func

Or from OCI Console:

Developer Services → Functions → Select Function → "Test"



---

🧹 Optional: Remove Local Image
```
docker rmi mytenancy.ocir.io/functions/pdf-unlock-func:v2
```

