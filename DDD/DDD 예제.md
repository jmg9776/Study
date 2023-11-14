2023-11-04
이 예시에서는 온라인 쇼핑 시스템을 가정하며, 주요 도메인으로는 '주문(Order)', '고객(Customer)', '제품(Product)', '결제(Payment)' 등이 있어요. 
각 도메인은 독립적인 비즈니스 로직과 관련된 엔티티, 서비스, 리포지토리를  제공할거에요.
### 예시 패키지 구조

```
com.mycompany.onlineshopping
├── domain
│   ├── order
│   │   ├── Order.java (엔티티)
│   │   ├── OrderItem.java (값 객체)
│   │   └── OrderRepository.java (리포지토리 인터페이스)
│   ├── customer
│   │   ├── Customer.java (엔티티)
│   │   ├── Address.java (값 객체)
│   │   └── CustomerRepository.java (리포지토리 인터페이스)
│   ├── product
│   │   ├── Product.java (엔티티)
│   │   └── ProductRepository.java (리포지토리 인터페이스)
│   └── payment
│       ├── Payment.java (엔티티)
│       ├── PaymentMethod.java (값 객체)
│       └── PaymentRepository.java (리포지토리 인터페이스)
├── application
│   ├── order
│   │   ├── OrderService.java (응용 서비스)
│   │   └── OrderDTO.java (데이터 전송 객체)
│   ├── customer
│   │   ├── CustomerService.java (응용 서비스)
│   │   └── CustomerDTO.java (데이터 전송 객체)
│   ├── product
│   │   ├── ProductService.java (응용 서비스)
│   │   └── ProductDTO.java (데이터 전송 객체)
│   └── payment
│       ├── PaymentService.java (응용 서비스)
│       └── PaymentDTO.java (데이터 전송 객체)
├── infrastructure
│   ├── persistence
│   │   ├── OrderRepositoryImpl.java (리포지토리 구현)
│   │   ├── CustomerRepositoryImpl.java (리포지토리 구현)
│   │   ├── ProductRepositoryImpl.java (리포지토리 구현)
│   │   └── PaymentRepositoryImpl.java (리포지토리 구현)
│   └── util
│       └── Logger.java (로깅 유틸리티)
└── presentation
    ├── web
    │   ├── OrderController.java (컨트롤러)
    │   ├── CustomerController.java (컨트롤러)
    │   ├── ProductController.java (컨트롤러)
    │   └── PaymentController.java (컨트롤러)
    └── dto
        ├── OrderResponse.java (뷰 모델)
        ├── CustomerResponse.java (뷰 모델)
        ├── ProductResponse.java (뷰 모델)
        └── PaymentResponse.java (뷰 모델)

```
### 설명

- **도메인 계층 (Domain)**: 각 도메인은 엔티티, 값 객체, 리포지토리 인터페이스를 포함할거에요. 예를 들어, '주문' 도메인에는 `Order`, `OrderItem` 등의 엔티티와 `OrderRepository` 인터페이스가 포함해요.
- **응용 계층 (Application)**: 각 도메인의 비즈니스 로직을 구현한 서비스와, 다른 계층으로 데이터를 전송할 때 사용하는 DTO가 여기에 포함되요.
- **인프라 계층 (Infrastructure)**: 각 도메인의 리포지토리 구현체와 기타 인프라 관련 코드(예: 데이터베이스 연결, 로깅)가 포함되요.
- **프레젠테이션 계층 (Presentation)**: 사용자 인터페이스와 관련된 컨트롤러와 뷰 모델이 이곳에 위치해요.
### 중요 포인트
- 각 도메인은 독립적으로 관리되어 시스템의 모듈화를 촉진하고 유지보수를 용이하게 해요.
- 코드는 도메인 중심으로 구성되어 있어, 비즈니스 로직의 이해와 개발이 용이해요.
- 시스템의 각 부분이 명확하게 분리되어 있어, 각 계층이나 모듈의 확장 및 수정이 용이해요.