# Terraform 기초
1. plan 수행 시 '+/-' 접두사와 '~' 접두사의 의미를 설명해주세요.
2. apply를 진행할 때, 코드에 변경한 내용이 없음에도 특정 리소스를 강제로 삭제 후 재생성하는 명령어를 작성해주세요.
    ```bash
    $terraform apply <option>=<resource>
    ```
3. 다음과 같은 버전 제약 구문의 의미를 작성해주세요.
    - \>= 1.0.0
    - ~> 1.0.0
4. plan을 매번 실행하지 않고 리소스의 값 또는 함수 결과를 확인하는 명령어는 무엇인가요?
5. Explicit Dependency를 지정할 때 사용하는 키워드는 무엇인가요?
6. 각 수명주기 키워드를 간략하게 설명해주세요.
    - prevent_destroy
    - ignore_changes
    - precondition
7. 다음 명령어들을 수행하면 어떤 일이 발생하는지 설명해주세요.
    - terraform init -upgrade
    - terraform fmt
    - terraform show
    - terraform destroy -auto-approve
    - terraform state rm \<resource>
8. plan을 수행할 때 가장 상세한 로그 내용을 보려면 어떻게 해야 할까요?
9. 다음 변수 입력 방식들을 먼저 적용되는 순서로 정렬해주세요.
    - terraform.tfvars
    - export TF_VAR_\<name>=\<value>
    - terraform plan -var=\<name>=\<value>
    - a.auto.tfvars
    - variable.tf 내 default 값
10. 다음 값들의 용도를 간략하게 설명해주세요.
    - data
    - variable
    - output
    - locals
11. 다음 조건에 부합하는 구성파일을 작성해주세요.
    - variable.tf 내 image_name의 글자수가 4글자 이상이고, 'ami-'로 시작하는 값이 아닌 경우 오류를 발생시킨다.