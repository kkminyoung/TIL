# MySQL 설계 및 세팅
졸프 db
```SQL
CREATE DATABASE `magicbox_flask`;
USE `magicbox_flask`;

CREATE TABLE `user` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `email` VARCHAR(255) NOT NULL unique,
  `password` VARCHAR(32) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `email_UNIQUE` (`email` ASC)
  );

CREATE TABLE `full` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `full_video` VARCHAR(50) NOT NULL,
  `date` DATETIME NOT NULL,
  `size` VARCHAR(45) NOT NULL,
  `storage_path` VARCHAR(250) NOT NULL,
  `user_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  FOREIGN KEY (`user_id`)
  REFERENCES `blackbox`.`user` (`id`)
  ON DELETE CASCADE
  ON UPDATE CASCADE
  );

CREATE TABLE `edited` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `edited_video` VARCHAR(50) NOT NULL,
  `edited_date` DATETIME NOT NULL,
  `user_id` INT NOT NULL,
  `full_id` INT NOT NULL,
  PRIMARY KEY (`id`, `full_id`),
  FOREIGN KEY (`user_id`)
  REFERENCES `blackbox`.`user` (`id`)
  ON DELETE CASCADE
  ON UPDATE CASCADE,
  FOREIGN KEY (`full_id`)
  REFERENCES `blackbox`.`full` (`id`)
  ON DELETE NO ACTION
  ON UPDATE NO ACTION
  );
  ```
  
  ![image](https://user-images.githubusercontent.com/61506233/105576740-3fc25c80-5db8-11eb-87a7-cd82807cadbb.png)


이제 MySQL 세팅은 끝났다! 연동을 해보자

# SQLAlchemy
파이썬 코드에서 Database와 연결하기 위해 사용할 수 있는 라이브러리중 SQLAlchemy라는 라이브러리가 있다.

SQLAlchemy는 ORM(Object Relational Mapper)로 관계형 데이터베이스의 테이블들을 프로그래밍 언어의 클래스로 표현할 수 있게 해주는 라이브러리이다.
즉, 클래스(class)를 사용해서 DB TABLE들을 표현하고 저장, 읽기, 업데이트, 삭제 등을 할 수 있도록 해준다.

# 설치
- 라이브러리 설치 과정에서 충돌이 났었다!
```
pip install sqlalchemy 
pip install mysql-connector-python // MySQL용 DBAPI
```

# API-MySQL 연결
### config.py 생성
- 우선 이부분을 수정해야했다! 팀원계정에 맞게 등록되어있는 정보를 내 MySQL에 맞게 수정해주어야함!!
```python
# config.py
db = {
    'user'     : 'root',		# 데이터베이스에 접속할 사용자 id
    'password' : 'root',		# 데이터베이스 사용자의 비밀번호
    'host'     : 'localhost',	 # 접속할 데이터베이스의 주소.
    'port'     : 3306,			# 접속할 데이터베이스의 포트(port)번호. MySQL일 경우 보통 3306포트를 사용한다.
    'database' : 'magicbox_flask'		# 실제 연결하고자 하는 데이터베이스 명

DB_URL = f"mysql+mysqlconnector://{db['user']}:{db['password']}@{db['host']}:{db['port']}/{db['database']}?charset=utf8"
```
### app.py 수정
config.py를 만들었으면 app.py를 수정하여 config.py의 데이터베이스 설정을 가져와서 연결한다.
그리고 sqlalchemy의 create_engine을 사용하여 데이터베이스 연결한다.

```python
from flask      import Flask, request, jsonify, current_app
from sqlalchemy import create_engine, text

def create_app(test_config = None):		# 1)
    app = Flask(__name__)

    app.json_encoder = CustomJSONEncoder

    if test_config is None:		# 2)
        app.config.from_pyfile("config.py")
    else:
        app.config.update(test_config)

    database     = create_engine(app.config['DB_URL'], encoding = 'utf-8', max_overflow = 0)		# 3)
    app.database = database		# 4)
    
    return app		# 5)
```


```
1) create_app이라는 함수를 정의한다.
Flask가 create_app이라는 이름의 함수를 자동으로 팩토리(factory) 함수로 인식해서 해당 함수를 통해 Flask를 실행시킨다.
그리고 create_app 함수가 test_config 인자를 받는다.
test_config는 단위 테스트(unit test)를 실행시킬 때 테스트용 데이트베이스 등의 테스트 설정 정보를 적용하기 위함이다.

2) 만일 test_conig인가자 None이면 config.py파일에서 설정을 읽는다.
만일 test_config 인자가 None이 아니라면, 즉, test_config값이 설정되어 들어왔다면, 단위 테스트를 실행 할 수 있다.

3) sqlalchemy의 create_engine 함수를 사용해서 데이터베이스의 연결을 한다.

4) 3)에서 생성한 Engine 객체를 Flask 객체에 저장함으로써 create_app 함수를 외부에서도 데이터베이스를 사용할 수 있도록 한다.

5) Flask 객체를 리턴한다. 앞서 create_app이라는 함수는 Flask가 자동 인지하여 Flask객체를 찾아서 실행될 수 있다.
```
