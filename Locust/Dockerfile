FROM locustio/locust

WORKDIR /app
COPY locustfile.py .
EXPOSE 8089
ENTRYPOINT [ "locust" ]
CMD [ "-f", "/app/locustfile.py" ]