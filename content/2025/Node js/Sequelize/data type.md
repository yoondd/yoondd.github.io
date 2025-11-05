Sequelize에서 사용 가능한 모든 데이터 타입을 MySQL을 기준으로 정리해보자.

## **문자열 (String)**

|Sequelize 데이터 타입|MySQL 자료형|설명|
|---|---|---|
|`DataTypes.STRING`|`VARCHAR(255)`|기본 문자열. 길이를 지정할 수 있음.|
|`DataTypes.STRING(50)`|`VARCHAR(50)`|지정된 길이의 문자열.|
|`DataTypes.STRING.BINARY`|`VARCHAR BINARY`|이진 문자열.|
|`DataTypes.CHAR`|`CHAR(255)`|고정 길이 문자열.|
|`DataTypes.CHAR(10)`|`CHAR(10)`|지정된 길이의 고정 문자열.|
|`DataTypes.TEXT`|`TEXT`|긴 텍스트 데이터.|
|`DataTypes.TEXT('tiny')`|`TINYTEXT`|최대 255 바이트의 작은 텍스트.|
|`DataTypes.TEXT('medium')`|`MEDIUMTEXT`|최대 16MB의 중간 크기 텍스트.|
|`DataTypes.TEXT('long')`|`LONGTEXT`|최대 4GB의 큰 텍스트.|

## **숫자 (Number)**

|Sequelize 데이터 타입|MySQL 자료형|설명|
|---|---|---|
|`DataTypes.INTEGER`|`INTEGER`|정수형 데이터.|
|`DataTypes.INTEGER.UNSIGNED`|`INTEGER UNSIGNED`|음수가 없는 정수형.|
|`DataTypes.BIGINT`|`BIGINT`|큰 정수형 데이터.|
|`DataTypes.FLOAT`|`FLOAT`|부동소수점 숫자.|
|`DataTypes.FLOAT(11, 10)`|`FLOAT(11,10)`|소수점 이하 자릿수를 지정한 부동소수점.|
|`DataTypes.DOUBLE`|`DOUBLE`|더 높은 정밀도의 부동소수점 숫자.|
|`DataTypes.DECIMAL(10, 2)`|`DECIMAL(10,2)`|고정 소수점 숫자.|

## **날짜 및 시간 (Date and Time)**

|Sequelize 데이터 타입|MySQL 자료형|설명|
|---|---|---|
|`DataTypes.DATE`|`DATETIME`|날짜와 시간을 포함한 데이터.|
|`DataTypes.DATE(6)`|`DATETIME(6)`|최대 6자리 소수 초를 지원하는 날짜/시간.|
|`DataTypes.DATEONLY`|`DATE`|날짜만 포함 (시간 없음).|

## **불리언 (Boolean)**

|Sequelize 데이터 타입|MySQL 자료형|설명|
|---|---|---|
|`DataTypes.BOOLEAN`|`TINYINT(1)`|참/거짓 값을 저장하는 불리언 타입.|

## **고유 식별자 (UUID)**

|Sequelize 데이터 타입|MySQL 자료형|설명|
|---|---|---|
|`DataTypes.UUID`|`CHAR(36)`|고유 식별자(UUID).|

## **이진 데이터 (Binary Data)**

|Sequelize 데이터 타입|MySQL 자료형|설명|
|---|---|---|
|`DataTypes.BLOB`|`BLOB`|바이너리 데이터를 저장하는 타입.|
|`DataTypes.BLOB('tiny')`|`TINYBLOB`|작은 크기의 바이너리 데이터.|

## **열거형 (Enum)**

|Sequelize 데이터 타입|MySQL 자료형|설명|
|---|---|---|
|`DataTypes.ENUM('a', 'b')`|`ENUM('a', 'b')`|지정된 값 중 하나를 선택하는 열거형 타입.|
