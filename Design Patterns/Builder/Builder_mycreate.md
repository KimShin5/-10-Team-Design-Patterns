## 캐릭터를 생성하는 빌더 오픈소스 예제 제작

```c++
#include <iostream>
#include <string>

using namespace std;

//Product-생성할 객체
class Character {
private:
	string inv_;
	string name_;
	int hp_;
	int mp_;
	int lv_;
public:
	//캐릭터에게 필요한 것들
	void setInven(const string& inv) {
		inv_ = inv;
	}
	void setName(const string& nam) {
		name_ = nam;
	}
	void setHP(const int& hp) {
		hp_ = hp;
	}
	void setMP(const int& mp) {
		mp_ = mp;
	}
	void setLV(const int& lv) {
		lv_ = lv;
	}

	//출력용 get- 함수들
	string getInven() const {
		return inv_;
	}
	string getName() const {
		return name_;
	}
	int getHP() const {
		return hp_;
	}
	int getMP() const {
		return mp_;
	}
	int getLV() const {
		return lv_;
	}
};

//Builder-객체 생성 단계 추상화
class charactercreate {
public:
	//객체 설정 가상함수
	virtual void buildINV() = 0;
	virtual void buildNAM() = 0;
	virtual void buildHP() = 0;
	virtual void buildMP() = 0;
	virtual void buildLV() = 0;
	//최종 캐릭터 객체 반환 가상함수
	virtual Character getcreate() const = 0;
};

//ConcreateBuilder-빌더 인터페이스 구현, 실제 객체 구현
class NewCharacter : public charactercreate {
private:
	Character minsu;
public:
	//객체 값 설정
	void buildINV() override {
		minsu.setInven("200칸");
	}
	void buildNAM() override {
		minsu.setName("민수");
	}
	void buildHP() override {
		minsu.setHP(100);
	}
	void buildMP() override {
		minsu.setMP(200);
	}
	void buildLV() override {
		minsu.setLV(1);
	}

	Character getcreate() const override {
		return minsu;
	}
};

//Directer-빌더를 통해 객체 생성 방법을 정의, getcreate를 통해 캐릭터 생성
class characterDirector {
public:
	Character construct(charactercreate& create){
		create.buildINV();
		create.buildNAM();
		create.buildHP();
		create.buildMP();
		create.buildLV();
		return create.getcreate();
	}
};

int main(int argc, char const argv[]) {
	characterDirector director; //디렉터 호출
	NewCharacter minsu;

	Character user1 = director.construct(minsu);

	cout << "캐릭터가 생성되었습니다 : " << endl;
	cout << "이름 : " << user1.getName() << endl;
	cout << "인벤토리: " << user1.getInven() << endl;
	cout << "체력: " << user1.getHP() << endl;
	cout << "마나: " << user1.getMP() << endl;
	cout << "레벨: " << user1.getLV() << endl;

	return 0;
}
```

출력 값은 이렇다
```
캐릭터가 생성되었습니다 :
이름 : 민수
인벤토리: 200칸
체력: 100
마나: 200
레벨: 1
```