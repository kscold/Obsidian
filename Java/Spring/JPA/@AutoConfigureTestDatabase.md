- @AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE) 이 어[[어노테이션(Annotation)]]을 통해 TestDB를 안쓴다고 명시하는 것이다.
- 기본값은 ANY이다.

- 이 [[어노테이션(Annotation)]]을 붙이 클래스에서만 사용하지 않게 만드는 방법 말고 application.[[yaml]] 파일에서 전역설정을 할 수 있다.

```yaml
test.database.replace: none # 문서화가 안되어 있기에 각별히 확인이 필요함
```