FROM python:3.10

WORKDIR /usr/src/app

# Instala dependencias
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copia el código del proyecto
COPY . .

# Corre el microservicio
CMD ["python", "app.py"]

EXPOSE 5000