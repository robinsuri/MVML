### Deploy: Docker

**Docker Container**

1. Check out the [uwsgi-nginx-flask-docker](https://github.com/tiangolo/uwsgi-nginx-flask-docker) image documentation
2. Make sure your app conforms to the exact `app/` > `main.py` > `app()` structure as specified above
3. Create a `Dockerfile` with:

```sh
FROM tiangolo/uwsgi-nginx-flask:python3.8

COPY ./app /app
RUN pip install -r requirements.txt
```

4. Test it locally:

```sh
cd app &&\
export FLASK_APP=main &&\
export FLASK_RUN_PORT=8000 &&\
export FLASK_ENV=development &&\
flask run
```

**Initial Server Setup**

5. Spin up a DigitalOcean Docker Image from the "Marketplace"

6. ssh into the machine:

```sh
ssh root@142.93.XXX.104
```

7. (Optional) Update everything:

```sh
sudo apt update
sudo apt -y upgrade
```

8. (Optional) Get "new" monitoring:

```sh
curl -sSL https://repos.insights.digitalocean.com/install.sh | sudo bash # skip
```

9. Install `make` and `unzip`:

```sh
sudo apt install make unzip
```

10. Create a new user:

```sh
adduser mvml
```

> Enter password
>
> Leave everything else blank
>
> Enter Y

11. Adjust new user permissions:

```sh
usermod -aG sudo mvml
sudo usermod -aG docker mvml
rsync --archive --chown=mvml:mvml ~/.ssh /home/mvml
```

12. Sign in with the new user:

```sh
ssh mvml@142.93.XXX.104
```

**Download the App to the Server**

13. If your app is hosted on GitHub you can download it with:

```sh
wget https://github.com/maxhumber/mvml/archive/master.zip
```

14. Unzip and rename (I've called this app `oranginator`):

```sh
unzip master.zip
mv MVML-master oranginator
rm -f master.zip
```

**Build and Start Docker**

15. Move into the app:

```sh
cd oranginator
```

16. Build the app image:

```sh
docker build -t appimage .
```

17. Run the app:

```sh
docker run -d --name appcontainer -p 80:80 appimage
```

18. See if it works by visiting the server IPv4 address!



**Tear Down**

If you screwed up and need to kill the container:

1. Stop the container:

```sh
docker stop appcontainer
```

2. And delete the image:

```sh
docker rmi -f appimage &&\
docker rm appcontainer
```