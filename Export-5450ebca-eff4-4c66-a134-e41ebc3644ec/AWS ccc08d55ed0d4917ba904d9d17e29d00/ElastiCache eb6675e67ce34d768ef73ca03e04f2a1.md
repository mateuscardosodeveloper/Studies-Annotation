# ElastiCache

- Duas opções disponíveis: Redis e Memcached
- Caches são Banco de Dados na memória RAM, alta perfomace e baixa latência.
- Ajuda a reduzir a carga no Banco de Dados principal para leitura intensa
- Ajuda a fazer sua aplicação Stateless
- Escrita escalona usando Sharding
- Leitura escolona usando Read Replica
- Multi AZ (Avalability Zone) com Capacidade Failover

![Screenshot from 2022-05-27 21-39-20.png](ElastiCache%20eb6675e67ce34d768ef73ca03e04f2a1/Screenshot_from_2022-05-27_21-39-20.png)

# ElastiCache - Guarda Sessão de Usuário

![Screenshot from 2022-05-27 21-42-16.png](ElastiCache%20eb6675e67ce34d768ef73ca03e04f2a1/Screenshot_from_2022-05-27_21-42-16.png)

### Redis

- Redis é um Banco de Dados do tipo Key-Value que armazena os dados na Ram
- Latência muito baixa (sub ms)
- Cache não se perde quando instância é reiniciada (persistente)
- Ideal para:
    - Sessão do Usuário
    - Alivia carga no Banco de Dados (ex. RDS)
- Muilt AZ com failover automáticamente para recuperação de desastres para não perca de dados.
- Suporta Read Replicas

### Memcached

- Memcached armazena objetos na memória RAM
- Cache perde os dados se for reiniciado
- Leitura rápida dos objetos armazenados
- AWS não vai perguntar qual é o melhor