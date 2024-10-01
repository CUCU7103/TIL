# 의존성이란?
- 의존성은 객체 지향 프로그래밍에서 클래스나 모듈간의 관계를 의미합니다.
- A가 B를 의존한다라는 말은 A는 B를 필요로 한다와 동일한 의미를 가집니다.
  - 의존대상 B에 변경사항이 생기면, 그것이 A에 영향을 미칩니다.
  - 의존대상 B가 변하면 A에 문제가 발생할 확률이 높아집니다.

클라이언트 클래스는 현재 햄버거 셰프에 의존하고 있습니다.
햄버거 셰프에 변경사항이 생기거나 다른 셰프로 변경하려면 클라이언트에 영향을 미칩니다.
즉 클라이언트는 햄버거 셰프에게 의존하고 있다고 볼 수잇습니다.
```
public class Client {
    private HamBurgerChef hamBurgerChef;

    public Client() {
       this.hamBurgerChef = new HamBurgerChef();
        hamBurgerChef.chefName();
        hamBurgerChef.cooking();
        hamBurgerChef.tasting();
        hamBurgerChef.cookingComplete();
    }
}

===========================

public class HamBurgerChef {

    private String name = "버거 마스터";

    public void cooking() {
        System.out.println("햄버거를 조리합니다.");
    }

    public void tasting() {
        System.out.println("조리된 햄버거를 맛봅니다.");
    }

    public void cookingComplete() {
        System.out.println(name + "가 조리를 마무리 하였습니다.");
    }

    public void chefName() {
        System.out.println(name);
    }
}
==========================
public class MainEx1 {

    public static void main(String[] args) {
        Client client = new Client();

    }
}

```


# 의존성 주입이란?
- 의존관계를 외부에서 결정하고 주입하는 것을 의미합니다.
- 위의 코드는 내부적으로 클라이언트 내에서 어떤 Chef를 사용할지를 정하고 있습니다
- 이때 Chef를 정하는것을 외부에서 결정하도록 하는 것입니다.

위의 코드를 개선해보겠습니다.

여러 분야의 셰프를 받을 수 있도록 인터페이스를 통해 추상화 하여 다형성을 사용하고,
클라이언트 내부에서 셰프를 지정하는 것을 외부에 별도의 클래스를 생성하여 받도록 하겠습니다.

```
public interface Chef {

    void cooking();

    void tasting();

    void chefName();

    void cookingComplete();

}

public class HamBurgerChef implements Chef {

    private String name = "버거 마스터";

    @Override
    public void cooking() {
        System.out.println("햄버거를 조리합니다.");
    }

    @Override
    public void tasting() {
        System.out.println("조리된 햄버거를 맛봅니다.");
    }

    @Override
    public void cookingComplete() {
        System.out.println(name + "가 조리를 마무리 하였습니다.");
    }

    @Override
    public void chefName() {
        System.out.println(name);
    }
}

public class SaladChef implements Chef {

    private String name = "셀러드 마스터";

    @Override
    public void cooking() {
        System.out.println("샐러드를 조리합니다.");
    }

    @Override
    public void tasting() {
        System.out.println("조리한 셀러드를 맛봅니다");
    }

    @Override
    public void cookingComplete() {
        System.out.println(name + "가 조리를 마무리 하였습니다.");
    }

    @Override
    public void chefName() {
        System.out.println(name);
    }
}
=================================================
// 생성자 주입 방식
public class FoodProvider {
    private Chef chef;

    public FoodProvider(Chef chef) {
        this.chef = chef;
    }

    void ChooseChef(){
        chef.chefName();
        chef.cooking();
        chef.tasting();
        chef.cookingComplete();
    }
}

//setter Method 방식
public class FoodProvider2 {

    private Chef chef;


    public void setChef(Chef chef){
        this.chef = chef;
    }

    void ChooseChef(){
        chef.chefName();
        chef.cooking();
        chef.tasting();
        chef.cookingComplete();
    }

}
=========================================================
public class Client {
    public static void main(String[] args) {
        SaladChef saladChef = new SaladChef();
        HamBurgerChef hamBurgerChef = new HamBurgerChef();

        FoodProvider foodProvider = new FoodProvider(saladChef);
        foodProvider.ChooseChef();

        System.out.println();

        FoodProvider2 foodProvider2 = new FoodProvider2();
        foodProvider2.setChef(new HamBurgerChef());
        foodProvider2.ChooseChef();

    }
}

```
이제 클라이언트는 셰프를 선택하기만 하면 됩니다.<br> 
즉 클라이언트 셰프 변경시 클래스의 코드를 수정할 일이 없거나 기존 방식보다 줄어들게 됩니다.

DI의 장점
1. 의존성이 줄어듭니다.
2. 재사용성이 높아집니다.
3. 테스트하기 좋은 코드가 됩니다.
4. 가독성이 높아집니다.














