```c++
#include<stdlib.h>
#include<iostream>
#include<stack>

using namespace std;
class Calculator{
public:
	//构造函数
	Calculator()
	{}
	void Run();//执行表达式计算
	void clear();//清除栈空间

private:
	stack<double> s;
	void AddOperand(double value);//操作数进栈
	bool Get2Operand(double& left, double& right);//两个操作数出栈
	void DoOperand(char op);//形成运算指令，进行计算
};


//操作数进栈
void Calculator::AddOperand(double value){
	s.push(value);//将操作数进栈  
}

//两个操作数出栈
bool Calculator::Get2Operand(double& left, double& right){
	if (s.empty() == true)
	{
		cout << "缺少右操作数！" << endl;
		return false;
	}
	right = s.top();
	s.pop();
	if (s.empty() == true)
	{
		cout << "缺少左操作数!" << endl;
		return false;
	}
	left = s.top();
	s.pop();
	return true;
}

//形成运算指令，进行计算
void Calculator::DoOperand(char op){
	double left, right;//待计算的左值和右值
	double result;//计算的结果
	if (Get2Operand(left, right) == true){
		switch (op){
		case '+':
			result = left + right;
			s.push(result);
			break;
		case '-':
			result = left - right;
			s.push(result);
			break;
		case '*':
			result = left * right;
			s.push(result);
			break;
		case '/':
			if (right == 0.0){
				cerr << "右值为零！" << endl;
				clear();
			}
			else{
				result = left / right;
				s.push(result);
			}
			break;
		default:
			clear();
		}
	}
	else
		clear();
}

//读字符串并求一个后缀表达式的值，以字符'#'结束
void Calculator::Run(){
	char ch;
	double newOperand;
	while (cin >> ch, ch != '='){
		switch(ch){
		case'+':case'-':case'*':case'/':
			DoOperand(ch);
			break;
		default:
			cin.putback(ch);
			cin >> newOperand;
			AddOperand(newOperand);
		}
	}
	if (ch == '=')
	{
		newOperand = s.top();
		cout << newOperand << endl;
	}

}

//清除栈空间
void Calculator::clear(){
	s.empty();
}







int main()
{
	Calculator c;
	while (1){
		cout << "请输入操作数和操作符：" << endl;
		c.Run();
		c.clear();
		fflush(stdin);
	}

	return 0;
}
```
