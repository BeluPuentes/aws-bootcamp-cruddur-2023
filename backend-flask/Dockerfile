FROM python:3.10-slim-buster AS builder

# Inside Container
# make a new folder inside cotainer
WORKDIR /backend-flask

# Outside Container -> Inside Container
# this contains the libraries want to install to run the app
COPY requirements.txt requirements.txt
# Inside Container
# install the python libraries for the app
RUN pip3 install --user --no-cache-dir -r requirements.txt

# Outside Container -> Inside Container
# . means everything in the curren directory
# first period . - /backend-flask (outside container)
# second period . /backend-flask (inside container)
COPY . .

FROM python:3.10-slim-buster

WORKDIR /backend-flask

COPY --from=builder /root/.local /root/.local
ENV PATH=/root/.local/bin:$PATH

#Flask.sh
COPY flask.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/flask.sh

RUN apt-get update 
RUN apt-get install -y gcc
RUN apt-get install -y curl

# Set Environment Variables (Env Vars)
# inside Container and will remin set when the container is running
ENV FLASK_ENV=development

EXPOSE ${PORT}
CMD [ "/usr/local/bin/flask.sh"]

# CMD (command)
#python3 -m flask run --host=0.0.0.0 --port=4567
#CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]
#COPY run_flask.sh /usr/local/bin/
#RUN chmod +x /usr/local/bin/flask.sh

