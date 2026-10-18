FROM redis:8.10.2

RUN apt-get update
RUN apt-get install -y locales locales-all

CMD ["sh", "-c", "exec redis-server --appendonly yes --requirepass \"$REDIS_PASSWORD\""]