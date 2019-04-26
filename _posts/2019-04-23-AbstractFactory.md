---
title: "Abstract Factory Pattern"
date: 2019-04-23 20:00:00 +0900
categories: Gof Study
---

# Abstract Factory Pattern

### 추상 팩토리 패턴
인터페이스를 이용하여, 서로 의존적이거나 연관된 객체를 일관되게 생성할 수 있다.   
UML  
![](https://github.com/studyteamthree/GofStudy/blob/master/images/Abstract_factory_UML.svg.png)[1]  

[1]:https://ko.wikipedia.org/wiki/%EC%B6%94%EC%83%81_%ED%8C%A9%ED%86%A0%EB%A6%AC_%ED%8C%A8%ED%84%B4  

Abstract Factory
> Factory에 대한 공통 인터페이스 (추상체)

Concrete Factory 1, 2
>실제 객체를 생성하는 구상된 클래스

Product A1, A2, B1, B2
> 팩토리를 통해 생성된 구체적인 객체

Abstract Product A, B
> 생성할 객체들에 대한 공통 인터페이스

Client
> 사용자


### 예제
GUI객체를 만드는 예제  
버튼과 셋팅 두개의 기능을 제공한다고 가정.  
각각 Abstract Product A, B 에 해당
```java
//Click기능이 있는 버튼
public interface Button {
	public void Click();
}


//Setting창을 열어주는 SettingView
public interface SettingView {
	public void openSettingView();
}
```
각 OS별 객체 동작 구현(실제 방법 생략)  
구체적인 객체 Product A1, A2, B1, B2  에 해당
```java
public class MacButton implements Button {
	@Override
	public void Click() {
		//MAC API		
	}
}

public class MacSettingView implements SettingView{
	@Override
	public void openSettingView() {
		//MAC API		
	}
}

public class WindowsButton implements Button{
	@Override
	public void Click() {
		//WIN32 ...
	}
}

public class WindowsSetting implements SettingView{
	@Override
	public void openSettingView() {
		//WIN32API		
	}
}
```
위의 객체들은 일관되게 생성되어야 한다.
이를 위한 팩토리 클래스 사용  
추상체인 Abstract Factory 및
구상체 Concrete Factory 1, 2  에 해당
```java
//Fatory 인터페이스
public interface GuiFactory {
	public Button createButton();
	public SettingView createSettingView();
}

//Mac 용 GUI객체를 생성하는 Factory 클래스
public class MacGuiFactory implements GuiFactory{
	@Override
	public Button createButton() {
		// TODO Auto-generated method stub
		return new MacButton();
	}

	@Override
	public SettingView createSettingView() {
		// TODO Auto-generated method stub
		return new MacSettingView();
	}
}

//Windows 용 GUI객체를 생성하는 Factory 클래스
public class WindowsGuiFactory implements GuiFactory{
	@Override
	public Button createButton() {
		// TODO Auto-generated method stub
		return new WindowsButton();
	}

	@Override
	public SettingView createSettingView() {
		// TODO Auto-generated method stub
		return new WindowsSetting();
	}

}
```
실제 이용시 일관된 OS에 맞는 객체를 생성할 수 있으며, OS 변경시 osName값만 변경해주면 된다.

```java
public class Main {
	public static void main(String arg[])
	{
		GuiFactory guiFactory = null;
		
		String osName = "Win"; //enviroment.GetOsName();
		
		if (osName.equalsIgnoreCase("win"))
		{
			guiFactory = new WindowsGuiFactory();
		}
		else if (osName.equalsIgnoreCase("Mac"))
		{
			guiFactory = new MacGuiFactory();
		}
		//...
		
		Button btn = guiFactory.createButton();
		SettingView setView = guiFactory.createSettingView();
		
		btn.Click();
		setView.openSettingView();
	}
}
```
위 예제에서는 버튼과, 셋팅창 두개의 객체 만을 생성하지만 무수히 많은 종류의 객체가 필요한 경우 개발자의 실수를 야기할 가능성이 높다.
하지만 추상 팩토리 패턴을 적용함으로써 일관된 객체를 생성할 수 있다.
