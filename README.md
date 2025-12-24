# Лабораторная работа 1. Знакомство с IaaS, PaaS, SaaS сервисами в облаке на примере Amazon Web Services (AWS). Создание сервисной модели.

## 1. Цель работы

Научиться:

- понимать уровни абстракции облака
- Проанализировать биллинг AWS
- Классифицировать потребление

## 2. Исходные данные

`Mapping Rules AWS team1.csv` - содержит строки `Product Code` и `Usage Type`, по которым нужно заполнить столбцы.

`Product Code` - это идентификатор продукта или сервиса AWS в биллинге.

- AmazonS3
- AmazonSNS
- ...

`Usage Type` - это строковой шаблон, описывающий конкретный тип потребления или метрики.

- %Requests%Tier1
- %Requests%Tier2
- %SMS-Price%
- ...

`IaaS` - Infrastructure as a Service

`PaaS` - Platform as a Service

`SaaS` - Sofware as a Service

## 3. Ход работы

### 3.1. Импорт и анализ данных

Для каждой строки биллинга строилась иерархия:

`IT Tower → Service Family → Service Type → Service Sub Type → Service Usage Type`

- **IT Tower** — верхнеуровневая технологическая область
- **Service Family** — семейство сервисов внутри области
- **Service Type** — конкретный AWS сервис
- **Service Sub Type** — подтип потребления/функциональная категория
- **Service Usage Type** — самая детальная метрика биллинг

### 3.2. Анализ и разбор построчно 

В каждой строке:

- Определяли Сервис по `Product Type`
- Определяли Что за услуга по `Usage Type`
- Заполняли классификационные столбцы `Service Type`, `Service Sub Type`, `Service Usage Type`

## 4. Примеры заполнения таблицы

**Amazon S3 Requests**

| IT Tower | Service Family           | Service Type | Service Sub Type | Service Usage Type | Product Code | Usage Type      |
| -------- | ------------------------ | ------------ | ---------------- | ------------------ | ------------ | --------------- |
| Storage  | Storage&Content Delivery | Amazon S3    | Requests         | Tier 1 Requests    | AmazonS3     | %Requests%Tier1 |
| Storage  | Storage&Content Delivery | Amazon S3    | Requests         | Tier 2 Requests    | AmazonS3     | %Requests%Tier2 |
| Storage  | Storage&Content Delivery | Amazon S3    | Requests         | Tier 3 Requests    | AmazonS3     | %Requests%Tier3 |


**Amazon SNS Delivery Attempts**

| IT Tower       | Service Family          | Service Type | Service Sub Type          | Service Usage Type     | Product Code | Usage Type              |
| -------------- | ----------------------- | ------------ | ------------------------- | ---------------------- | ------------ | ----------------------- |
| Cloud Services | Application Integration | Amazon SNS   | Message Delivery          | HTTP Delivery Attempts | AmazonSNS    | %DeliveryAttempts-HTTP  |
| Cloud Services | Application Integration | Amazon SNS   | Message Delivery          | SQS Delivery Attempts  | AmazonSNS    | %DeliveryAttempts-SQS   |
| Cloud Services | Application Integration | Amazon SNS   | SMS Messaging             | SMS Price              | AmazonSNS    | %SMS-Price%             |
| Cloud Services | Application Integration | Amazon SNS   | SMS Messaging             | SMS Sent               | AmazonSNS    | %SMS-Sent%              |
| Cloud Services | Application Integration | Amazon SNS   | Mobile Push Notifications | APNS Delivery Attempts | AmazonSNS    | %DeliveryAttempts-APNS% |

**AI/ML** 

| IT Tower       | Service Family | Service Type      | Service Sub Type | Service Usage Type | Product Code | Usage Type       |
| -------------- | -------------- | ----------------- | ---------------- | ------------------ | ------------ | ---------------- |
| Cloud Services | Analytics      | Amazon Translate  | Text Translation | Translate Text     | translate    | %TranslateText   |
| Cloud Services | Analytics      | Amazon Transcribe | Speech to Text   | Streaming Audio    | transcribe   | %StreamingAudio  |
| Cloud Services | Analytics      | Amazon Transcribe | Speech to Text   | Transcribe Audio   | transcribe   | %TranscribeAudio |

**DevOps**

| IT Tower       | Service Family | Service Type     | Service Sub Type | Service Usage Type | Product Code    | Usage Type          |
| -------------- | -------------- | ---------------- | ---------------- | ------------------ | --------------- | ------------------- |
| Cloud Services | DevOps         | AWS CodePipeline | Additional Costs | Tax                | AWSCodePipeline | Tax%                |
| Cloud Services | DevOps         | AWS CodePipeline | Pipeline Usage   | Trial Pipeline     | AWSCodePipeline | %trialPipeline%     |
| DevOps         | DevOps         | AWS CodeBuild    | Build            | Build Execution    | CodeBuild       | (не указан) |


## 5. Итоговая классификация сервисов

**Storage:**

- **Amazon S3** (Requests Tier 1–6, Tag Storage/Tag Hours, Early Delete)
- **Amazon Glacier** (Capacity/Requests/Storage)

**Cloud Services/Analytics:**

- **Amazon Redshift** (Compute Node, Storage, Data Process)
- **Amazon Translate** (TranslateText)
- **Amazon Transcribe** (StreamingAudio, TranscribeAudio)
- **Amazon Machine Learning (AmazonML)** (TrainModel, EvaluateModel)
- **Amazon Polly** (Text-to-Speech / Synthesis)

**Cloud Services/Application Integration:**

- **Amazon SNS** (DeliveryAttempts HTTP/SQS/APNS, SMS Sent/Price)

**DevOps:**

* **AWS CodePipeline** (TrialPipeline, Tax)
* **AWS CodeBuild** (Build Execution)

## 6. Вывод 

В ходе лабораторной работы был выполнен анализ строк AWS Billing и построена сервисная классификация потребления облачных сервисов по иерархии “от общего к частному”

Поняли, что:

* `Product Code` однозначно определяет AWS-сервис,
* `Usage Type` определяет конкретный вариант потребления/метрику биллинга,
* сервисная модель позволяет агрегировать затраты на разных уровнях (например, от IT Tower Storage до конкретного Tier запросов S3),
* часть строк отражает дополнительные расходы (Tax) и должна выделяться отдельно,
* соблюдение единого стиля заполнения (Service Type = конкретный сервис) обеспечивает консистентность итоговой модели и удобство анализа.
