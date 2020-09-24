# AWS 회원가입 그리고 지역 선택

https://ap-northeast-2.console.aws.amazon.com/console/home?region=ap-northeast-2

![AWS_지역선택](./images/aws_region.png)

지역에 맞도록 선택하여 console을 접근합니다.

# 인스턴스 시작하기 & 만들기

![AWS_지역선택](./images/ec2_create.png)
![AWS_지역선택](./images/os.png)

우분투 OS를 사용할 것이므로 프리티어 사용가능한 우분투 OS를 선택해 줍니다. <br/>
Ubuntu Server 20.04 LTS (HVM), SSD Volume Type 선택해 줍니다.

![AWS_지역선택](./images/os_ca.png)

Ubuntu Server 버전 차이점을 확인하세요.

EC2 인스턴스 유형을 가격과 웹사이트 트래픽등 을 고려하여 적절하게 선택합니다.

[AWS EC2 나의 홈페이지에는 어떤 유형이 맞을까?](https://aws.amazon.com/ko/ec2/instance-types/) <br/>
[AWS EC2 Network 트래픽 예상해서 네트워크 사양 파악하기](https://tech.cloud.nongshim.co.kr/2018/11/13/aws-ec2-network%EC%84%B1%EB%8A%A5%EC%97%90-%EB%8C%80%ED%95%B4/) <br/>
[AWS EC2 요금 측정하기](https://calculator.aws/#/addService)

EC2 instance서버에 FTP서버용으로 windows Server2008버전의 OS를 사용하는 고객이 있습니다.
사양은 C4.large 였고 약 500MB용량의  파일을 약20,000명의 유저들이 한번에 다운로드 받아가는 대규모 업데이트를 진행하고자합니다.
업데이트는 오후 6시부터 진행하여 다음날 아침8시 까지 약 14시간안에는 다운로드가 완료가 되어야 하는 상황입니다.
위에서 주어진 상황에 얼만큼의 네트워크성능이(Gbps) 필요할까요?

정답은 아래와 같습니다. <br/>
500MB * 20,000명 = 약 9,766GB <br/>
14시간 = 14 * 60 * 60초 = 50,400초 <br/>
따라서 9,766 GB / 50,400초  = 0.19 GBytes/s = 1.55 Gbit/s 의 네트워크성능이 필요합니다.

# 인스턴스 접근하기

CND 관리자 권한으로 실행 후 pem 파일로 이동하여 ec2 연결

# PHP 설치
- 우분트 업데이트
    - sudo apt update

sudo apt install php php-cli php-fpm php-json php-pdo php-mysql php-zip php-gd  php-mbstring php-curl php-xml php-pear php-bcmath

sudo apt-get update
sudo apt -y install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update

sudo apt -y install php7.4

Step 1: sudo apt-get install -y php7.4-gd
Step 2: sudo apt-get install php7.4-intl
Step 3: sudo apt-get install php7.4-xsl
Step 4: sudo apt-get install php7.4-mbstring
Step 5: sudo apt-get install php7.4-zip 
sudo apt-get install php7.4-fpm
sudo apt-get install php7.4-mysql

스탭 5 가 핵심이였다.

# nginx 설치

sudo apt-get install nginx

~~~
cd /var/www/html
~~~

프로젝트 git 으로 가져오기

sudo npm install -g npm 으로 최신유지

# nginx 설정

cd /etc/nginx/sites-available/

~~~
root /var/www/html/payoneer/public;

# Add index.php to the list if you are using PHP
index index.php index.html index.htm index.nginx-debian.html;

server_name 3.34.190.32;

charset utf-8;

location / {
        # First attempt to serve request as file, then
        # as directory, then fall back to displaying a 404.
        try_files $uri $uri/ /index.php?$query_string;
}

error_page 404 /index.php;

location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
}

location ~ /\.(?!well-known).* {
        deny all;
}
~~~

~~~
sudo nginx -t
~~~ 
테스트 확인

~~~
sudo service apache2 stop -> sudo service nginx start
~~~
웹서버 교체

~~~
sudo composer install

sudo chown -R www-data:www-data storage/
sudo chown -R www-data:www-data public/
~~~
파일 권한 허용

~~~
sudo php artisan key:generate
~~~
키 생성

# 도메인 등록

[AWS Route53 도메인 생성](https://console.aws.amazon.com/route53/home#DomainListing:)

도메인을 생성합니다.

![AWS_지역선택](./images/route53_host.png)

[AWS Route53 호스팅 영역](https://console.aws.amazon.com/route53/v2/hostedzones#)

그 후 호스팅 영역에서 호스팅 영역 생성을 클릭합니다.

![AWS_지역선택](./images/route53_recod.png)

레코드 생성을 클릭하여 인스턴스 퍼블릭 ip 주소를 복사 붙여넣기 합니다.

페이오니아 홈페이지 사용한 라우트 레코드 셋 예제 (외부 도메인 연결 예제)
![AWS_지역선택](./images/recode-1.png)
![AWS_지역선택](./images/recode-2.png)
![AWS_지역선택](./images/recode-3.png)

아래는 구버전 등록방법

![AWS_지역선택](./images/route53_recod_y.png)

https://teddylee777.github.io/aws/%EC%95%84%EB%A7%88%EC%A1%B4AWS-%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4-%EB%8F%84%EB%A9%94%EC%9D%B8-%EC%97%B0%EA%B2%B0%ED%95%98%EA%B8%B0
https://wingsnote.com/57

# HTTPS 등록

![AWS_지역선택](./images/https.png)

# HTTPS 리다이렉트

![HTTPS_리다이렉트](./images/https_redirect.png)

# HTTPS 등록 다른 블로그 글

![HTTPS_리다이렉트](./images/https_add.png)

# HTTPS Load Balancer 생성

[로드 벨런서](https://ap-northeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LoadBalancers:sort=loadBalancerName)

1. Application Load Balancer
    - 1단계
        1. 리스너 : HTTP, HTTPS
        2. 가용 영역 : a,c
    - 2단계
        1. 기본 인증서 선택 : ACM에서 인증서 선택 (권장)
    - 3단계
        1. 사용하려는 EC2 보안그룹 선택
    - 4단계
        1. 라우팅 구성
    - 5단계
        1. 사용하는 EC2 선택
        
 
