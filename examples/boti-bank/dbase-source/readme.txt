Opcion arrancado
1)
docker run -d \
  --name mariadb_db \
  -e MARIADB_ROOT_PASSWORD=root \
  -e MARIADB_DATABASE=botibank \
  -e MARIADB_USER=pablo \
  -e MARIADB_PASSWORD=patata \
  -p 3306:3306 \
  -v mariadb_data:/var/lib/mysql \
  mariadb:lts

Pararlo: docker stop mariadb_db
docker rm -f mariadb_db

2) 
docker-compose up -d
docker-compose down

Borra todo: docker-compose down -v


PARA probar
create table cuentas (
  cuentaId  varchar(20)    not null,
  clienteId char(36)       not null,
  saldo     decimal(12,2)  not null default 0.00,
  tipo      enum('Corriente','Ahorro','Inversión','Sueldo') not null default 'Corriente',
  primary key (cuentaId),
  key idx_cuentas_cliente (clienteId)
) engine=InnoDB default charset=utf8mb4 collate=utf8mb4_unicode_ci;

insert into cuentas (cuentaId, clienteId, saldo, tipo) values
  ('CTA-122', '71992c72-cc1c-4c5a-8b50-9ee4fb6c214d', 1800.50, 'Corriente'),
  ('CTA-123', '71992c72-cc1c-4c5a-8b50-9ee4fb6c214d',  100.00, 'Ahorro'),
  ('CTA-999', '71992c72-cc1c-4c5a-8b50-9ee4fb6c214d',    0.00, 'Inversión');
  ('CTA-125', '71992c72-cc1c-4c5a-8b50-9ee4fb6c214d', 800.50, 'Corriente'),
  ('CTA-124', '71992c72-cc1c-4c5a-8b50-9ee4fb6c214d',  300.00, 'Ahorro'),
  ('CTA-995', '71992c72-cc1c-4c5a-8b50-9ee4fb6c214d',    1.00, 'Inversión');

  CURL:
  curl -s -X POST http://localhost:3002/mcp/messages \
  -H 'Content-Type: application/json' \
  -H 'X-API-Key: default-key-1235' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call",
       "params":{"name":"consultar_cuentas",
                 "arguments":{"cliente_id":"71992c72-cc1c-4c5a-8b50-9ee4fb6c214d"}}}' \
  | python3 -m json.tool


  curl -s -X POST http://localhost:3002/mcp/messages -H 'Content-Type: application/json' \
  -H 'X-API-Key: default-key-1235' -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}' | grep -c mariadb