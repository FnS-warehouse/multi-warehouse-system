server:
  port: 8181

logging:
  level:
    com.ecommerce.app: DEBUG

order-service:
  payment-request-topic-name: payment-request
  payment-response-topic-name: payment-response
  seller-approval-request-topic-name: seller-approval-request
  seller-approval-response-topic-name: seller-approval-response

spring:
  jpa:
    open-in-view: false
    show-sql: true
    database-platform: org.hibernate.dialect.PostgreSQL9Dialect
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQL9Dialect
  datasource:
    url: jdbc:postgresql://localhost:5433/postgres?currentSchema=order&binaryTransfer=true&reWriteBatchedInserts=true&stringtype=unspecified
    username: postgres
    password: admin
    driver-class-name: org.postgresql.Driver
    schema: classpath:init-schema.sql
  sql:
    init:
      platform: postgres
      mode: always

kafka-config:
  bootstrap-servers: localhost:19092,localhost:29092,localhost:39092
  schema-registry-url-key: schema.registry.url
  schema-registry-url: http://localhost:8081
  num-of-partitions: 3
  replication-factor: 3

kafka-producer-config:
  key-serializer-class: org.apache.kafka.common.serialization.StringSerializer
  value-serializer-class: io.confluent.kafka.serializers.KafkaAvroSerializer
  compression-type: none
  acks: all
  batch-size: 16384
  batch-size-boost-factor: 100
  linger-ms: 5
  request-timeout-ms: 60000
  retry-count: 5

kafka-consumer-config:
  key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
  value-deserializer: io.confluent.kafka.serializers.KafkaAvroDeserializer
  payment-consumer-group-id: payment-topic-consumer
  seller-approval-consumer-group-id: seller-approval-topic-consumer
  auto-offset-reset: earliest
  specific-avro-reader-key: specific.avro.reader
  specific-avro-reader: true
  batch-listener: true
  auto-startup: true
  concurrency-level: 3
  session-timeout-ms: 10000
  heartbeat-interval-ms: 3000
  max-poll-interval-ms: 300000
  max-poll-records: 500
  max-partition-fetch-bytes-default: 1048576
  max-partition-fetch-bytes-boost-factor: 1
  poll-timeout-ms: 150