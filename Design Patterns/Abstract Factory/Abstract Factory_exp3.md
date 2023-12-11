## 두번째 예시
```C++
#include <iostream>

using namespace std;

// 무기를 나타내는 추상 클래스
class Weapon {
public:
// 순수가상함수 파워 구현
    virtual int Power() = 0;
};

// 활 클래스: 무기의 구현
class Bow : public Weapon {
public:
    int Power() override {
        return 200;
    }
};

// 검 클래스: 무기의 구현
class Sword : public Weapon {
public:
    int Power() override {
        return 50;
    }
};//활과 검은 무기를 상속하여 각 각 50과 200의 파워를 반환시킴

// 갑옷을 나타내는 추상 클래스
class Armor {
public:
//순수가상함수 방어를 구현
    virtual int Defence() = 0;
};

// 가죽 갑옷 클래스: 갑옷의 구현
class Leather : public Armor {
public:
    int Defence() override {
        return 50;
    }
};

// 판금 갑옷 클래스: 갑옷의 구현
class Plate : public Armor {
public:
    int Defence() override {
        return 200;
    }
};// 가죽과 판금이 갑옷을 상속하여 각 각 50과 200의 방어를 반환한다.

// 장비를 생성하는 추상 팩토리 인터페이스
class EquipmentAbstractFactory {
public:
    virtual Weapon* CreateWeapon() = 0;
    virtual Armor* CreateArmor() = 0;
};//'EquipmentAbstractFactory' 순수 가상 클래스로 가상함수 정의

// 활과 가죽 갑옷을 생성하는 장비 팩토리
class ArcherEquipment : public EquipmentAbstractFactory {
public:
    Weapon* CreateWeapon() override {
        return new Bow();
    }

    Armor* CreateArmor() override {
        return new Leather();
    }
};//'EquipmentAbstractFactory'를 상속하여 활 및 가죽 생성

// 검과 판금 갑옷을 생성하는 장비 팩토리
class WarriorEquipment : public EquipmentAbstractFactory {
public:
    Weapon* CreateWeapon() override {
        return new Sword();
    }

    Armor* CreateArmor() override {
        return new Plate();
    }
};//'EquipmentAbstractFactory'를 상속하여 검 및 판금 생성

// 캐릭터를 나타내는 추상 클래스
class Character {
public:
    Character(EquipmentAbstractFactory* equipment) {
        mWeapon = equipment->CreateWeapon();
        mArmor = equipment->CreateArmor();
    }
//'Attack'가상함수 정의
    virtual void Attack() = 0;

protected:
    Weapon* mWeapon;
    Armor* mArmor;
};

// 활을 사용하는 궁수 클래스
class Archer : public Character {
public:
    Archer(EquipmentAbstractFactory* equipment) : Character(equipment) {}

    void Attack() override {
        cout << "활을 쏘아서 " << mWeapon->Power() << " 공격을 합니다." << endl;
    }
};//캐릭터를 상속, 'EquipmentAbstractFactory'를 통해 무기 초기화, Attack함수를 구현하여 파워에 따라 출력한다.

// 검을 사용하는 전사 클래스
class Warrior : public Character {
public:
    Warrior(EquipmentAbstractFactory* equipment) : Character(equipment) {}

    void Attack() override {
        cout << "검을 휘둘러서 " << mWeapon->Power() << " 공격을 합니다." << endl;
    }
};

int main() {
    //각 캐릭터의 객체 생성
    Character* archer = new Archer(new ArcherEquipment());
    Character* warrior = new Warrior(new WarriorEquipment());

    // 각 캐릭터의 공격 출력
    archer->Attack();
    warrior->Attack();

    // 메모리 해제
    delete archer;
    delete warrior;

    return 0;
}

오픈소스 링크: https://devjino.tistory.com/22