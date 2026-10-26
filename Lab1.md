openapi: 3.0.2
info:
  title: Test Automation Service
  description: Сервис тестирования. Лабораторная работа №1.
  version: 1.0.0
servers:
  - url: http://localhost:8080
    description: Локальный сервер
paths:
  /tests:
    get:
      summary: Получение всех тестов
      operationId: getAllTests
      parameters:
        - name: limit
          in: query
          description: Количество возвращаемых объектов. По умолчанию 100
          required: false
          schema:
            type: integer
            format: int64
        - name: offset
          in: query
          description: Сколько объектов от начала необходимо пропустить
          required: false
          schema:
            type: integer
            format: int64
      responses:
        '200':
          description: Список тестов
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Test'
        default:
          description: Непредвиденная ошибка
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
    post:
      summary: Создание теста
      operationId: createTest
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Test'
      responses:
        '201':
          description: Тест создан
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Test'
        default:
          description: Непредвиденная ошибка
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
  /tests/{id}:
    get:
      summary: Получение теста по id
      operationId: getTestById
      parameters:
        - name: id
          in: path
          required: true
          description: Id теста
          schema:
            type: string
            format: uuid
      responses:
        '200':
          description: Найденный тест
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Test'
        '404':
          description: Тест не найден
        default:
          description: Непредвиденная ошибка
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
    put:
      summary: Обновление теста по id
      operationId: updateTest
      parameters:
        - name: id
          in: path
          required: true
          description: Id теста
          schema:
            type: string
            format: uuid
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Test'
      responses:
        '200':
          description: Тест обновлён
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Test'
        '404':
          description: Тест не найден
        default:
          description: Непредвиденная ошибка
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
    delete:
      summary: Удаление теста по id
      operationId: deleteTest
      parameters:
        - name: id
          in: path
          required: true
          description: Id теста
          schema:
            type: string
            format: uuid
      responses:
        '204':
          description: Тест удалён
        '404':
          description: Тест не найден
        default:
          description: Непредвиденная ошибка
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
components:
  schemas:
    Test:
      type: object
      required:
        - id
        - title
        - subject
      properties:
        id:
          type: string
          format: uuid
        title:
          type: string
        description:
          type: string
        subject:
          type: string
        durationMinutes:
          type: integer
          format: int32
        maxScore:
          type: integer
          format: int32
        createdAt:
          type: string
          format: date-time
    Error:
      type: object
      required:
        - code
        - message
      properties:
        code:
          type: integer
          format: int32
        message:
          type: string
