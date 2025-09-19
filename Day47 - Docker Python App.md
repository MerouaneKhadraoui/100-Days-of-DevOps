# Day 47: Docker Python App

## Question

A python app needed to be Dockerized, and then it needs to be deployed on `App Server 2`. We have already copied a `requirements.txt` file (having the app dependencies) under `/python_app/src/` directory on `App Server 2`. Further complete this task as per details mentioned below:

**Task:**

1. Create a `Dockerfile` under `/python_app` directory:

  - Use any `python` image as the base image.
  - Install the dependencies using `requirements.txt` file.
  - Expose the port `6000`.
  - Run the `server.py` script using `CMD`.

2. Build an image named `nautilus/python-app` using this Dockerfile.

3. Once image is built, create a container named `pythonapp_nautilus`:

  - Map port `6000` of the container to the host port `8091`.

4. Once deployed, you can test the app using `curl` command on `App Server 2`.

`curl http://localhost:8091`

---

## Solution

1. **Go to the correct directory**

On **App Server 2**, move into the `/python_app` directory:

```bash
cd /python_app
```

2. **Create the Dockerfile**

Create a new file called `Dockerfile`:

```bash
vi Dockerfile
```

Add the following content:

```Dockerfile
# Use Python base image
FROM python:slim

# Set working directory
WORKDIR /app

# Copy requirements.txt first (for caching layers)
COPY src/requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application files
COPY src/ .

# Expose port 6000
EXPOSE 6000

# Run the app
CMD ["python", "server.py"]
```
Save and exit (`:wq`).

---

3. **Build the Docker image**

Run:

```bash
docker build -t nautilus/python-app .
```
This creates the image `nautilus/python-app`.

---

4. **Run the container**

Run the container with the correct name and port mapping:

```bash
docker run -d --name pythonapp_nautilus -p 8091:6000 nautilus/python-app
```
---

5. **Verify the container**

Check if it’s running:

```bash
docker ps
```
You should see something like:

```bash
CONTAINER ID   IMAGE                 COMMAND             PORTS                     NAMES
xxxxx          nautilus/python-app   "python server.py"  0.0.0.0:8091->6000/tcp   pythonapp_nautilus
```

---

6. **Test the app**

Run the `curl` test:

```bash
curl http://localhost:8091
```
If the app is correct, you should see its response.

---

✅ Done! Your Python app is dockerized, deployed, and accessible on **App Server 2** via port `8091`.