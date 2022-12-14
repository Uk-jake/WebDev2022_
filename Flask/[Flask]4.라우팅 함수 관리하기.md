# 라우팅 함수 관리하기

애플리케이션 팩토리를 이용한 코드를 참고하면 creat_app 함수 위에 @app.route('/')를 볼 수 있습니다.  
이러한 표시는 어노테이션이라고 부르고 `@app.route` 어노테이션은 함수와 URL을 매핑시켜주는 기능을 합니다.  

이러한 방법으로 새로운 URL매핑을 하기위해서는 라우팅이 필요할 때 마다 create_app함수를 계속 추가해줘야합니다.  
이러한 방식으로 함수를 계속 추가한다면 관리하기 어려워지는데 이런 문제를 해결하기 위해서 사용하는 것이 블루프린트입니다.

플라스크의 블루프린트를 이용하면 라우팅 함수를 체계적으로 관리할 수 있습니다.
블루프린트(blueprint)는 보통 객체지향 프로그래밍에서 "청사진"을 뜻하는 용어인데 플라스크에서는 URL과 함수의 매핑을 관리하기 위해 사용하는 도구(클래스)입니다.

### 블루프린트 생성
creat_app함수 안에 포함된 함수 대신 블루프린트를 사용할 수 있드록 수정해 봅시다.
블루프린트는 주로 views디렉토리 안에서 관리합니다. 그러므로 프로젝트 안에 views디렉토리를 만들고 그 안에 `main_views.py`파일을 만들어야합니다.

**main_views.py**
```
from flask import Blueprint

bp = Blueprint('main', __name__, url_prefix='/')


@bp.route('/')
def hello_pybo():
    return 'Hello, Pybo!'
```
여기서 주목해야할 것은 `@bp.route` 어노테이션입니다.
원래는 `@app.route`어노페이션을 사용했는데 여기서는 bp객체를 사용한 어노테이션을 사용했습니다.  
  
`bp = Blueprint('main', __name__, url_prefix='/')`  
Blueprint함수를 이용하여 bp객체를 선언하는 코드입니다.
첫번째 인수로 전달한 main은 블루프린트의 별칭입니다. 이 별칭은 나중에 자주 사용할 url_for함수에서 사용됩니다.
두번째 인수로 전달한 __name__은 모듈명인 main.views가 인수로 전달됩니다.  
세번째 인수 url_prefix는 라우팅 함수의 어노테이녓 URL앞에 기본으로 붙일 접두어 URL을 의미합니다.

### 블루프린트 등록
블루프린트를 사용하여 URL를 매핑하였으니 init파일로 돌아가 아래와 같이 수정을 해야합니다.
`__init__.py`

```
from flask import Flask

def create_app():
    app = Flask(__name__)

    from .views import main_views
    app.register_blueprint(main_views.bp)

    return app
```
이 후 컴파일을 하면 블루프린트가 잘 적용된 것을 볼 수 있습니다.

