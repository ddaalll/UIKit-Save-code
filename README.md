# UIKit-Save-code

버튼의 타이틀을 바꾸는 법

```swift
button.setTitle("text here", forState: .normal)
```

타이머 설정

```swift
weak var timer: Timer?

override func viewDidLoad(){
	super.viewDidLoad()
}

timer?.invalidate() //타이머를 중지하는 코드
timer = Timer.scheduledTimer(withTimeInterval: 1.0, repeats: true) { [self] _ in

//실행하고자 하는 코드 

}
```

사운드 출력 관련(시스템)

```swift
@import AVFoundation

AudioServicesPlayAlertSound(SystemSoundID(1322)) //(1322)시스템사운드 종류 선택
```

텍스트필드 사용법

```swift
textField.keyboardType = UIKeyboardType.emailAddress      //텍스트필드의 키보드 스타일
textField.placeholder = "e-mail"
textField.borderStyle = .roundedRect    //텍스트필드의 선 스타일
textField.clearButtonMode = .always     //텍스트필드의 모두 지우기 (x버튼 생성)
textField.returnKeyType = .go           //키패드 return 키 이름 바꾸기
```

텍스트필드 상세사용법(시점, 입력시작, 지워질때 등)

```swift
// 텍스트필드의 입력을 시작할때 호출 (시작할지 말지의 여부 허락하는 것)
    func textFieldShouldBeginEditing(_ textField: UITextField) -> Bool {
        print(#function)
        return true
    }
    
    // 시점 -
    func textFieldDidBeginEditing(_ textField: UITextField) {
        print(#function)
        print("유저가 텍스트필드의 입력을 시작했다.")
        
    }
    
    func textFieldShouldClear(_ textField: UITextField) -> Bool {
        print(#function)
        return true
    }
    
    
    // 텍스트필드 글자 내용이 (한글자 한글자) 입력되거나 지워질 때 호출이 되고 (허락) //글자수 제한 적용
    func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
        print(string)
        print(#function)
        return true
    }
    
    
    // 텍스트필드의 엔터키가 눌러지면 다음 동작을 허락할것인지 말것인지
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        print(#function)
        
        if textField.text == "" {
            textField.placeholder = "Type Something"
            return false
        } else {
            return true
        }
      
    }
    
    // 텍스트필드의 입력이 끝날떄 호출 (끝날지 말지를 허락)
    func textFieldShouldEndEditing(_ textField: UITextField) -> Bool {
        print(#function)
        return true
    }
    
    // 텍스트필드의 입력이 실제 끝났을때 호출 (시점)
    func textFieldDidEndEditing(_ textField: UITextField) {
        print(#function)
        print("유저가 텍스트필드의 입력을 끝냈다.")
    }
```

텍스트 필드에서, 글자 수를 제한하는 방법

```swift
func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {

let maxLength = 10
            let currentString = (textField.text ?? "") as NSString
            let newString = currentString.replacingCharacters(in: range, with: string)

            return newString.count <= maxLength
}
```

숫자 입력만 가능하도록하는 논리

```swift
func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
        
        if Int(string) != nil || string == "" {
            return true // 글자 입력을 허용
        }
        return false // 글자 입력을 허용하지 않음
        
    }
```

텍스트 필드의 글자수 + Int 작성 제한 (String만 입력 가능) 

- 입력되고 있는 글자가 “숫자”인 경우 입력을 허용하지 않는 논리

```swift
// 한글자 한글자 입력할때 마다 호출 ===> 참(입력하는 것을 허락) / 거짓 (입력하는 것을 거부)
if Int(string) != nil {   // (숫자로 변환이 된다면 nil이 아닐테니)
            return false
        } else {
						// 10글자이상 입력되는 것을 막는 코드
            guard let text = textField.text else {return true}
            let newLength = text.count + string.count - range.length
            return newLength <= 10   //글자 수 제한
        }
```

텍스트필드에서 화면의 탭을 감지하는 코드(화면 터치 시 키보드 내려감)

```swift
override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
    self.endEditing(true)
}
```

코드로 UI짜기

```swift
// 뷰를 기준으로 왼쪽 오토레이아웃 설정
emailTextFieldView.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 30).isActive = true

// 뷰를 기준으로 오른쪽 오토레이아웃 설정
emailTextFieldView.trailingAnchor.constraint(equalTo: view.trailingAnchor, constant: -30).isActive = true

// 뷰를 기준으로 상위 오토레이아웃 설정
emailTextFieldView.topAnchor.constraint(equalTo: view.topAnchor, constant: 200).isActive = true

// 뷰의 높이 설정
emailTextFieldView.heightAnchor.constraint(equalToConstant: 40).isActive = true
```

코드로 UI짜는 중, 오토레이아웃 설정 OFF 필수 ⭐️⭐️⭐️

```swift
emailTextFieldView.translatesAutoresizingMaskIntoConstraints = false
```

뷰를 둥글게 하는 법

```swift
emailTextFieldView.layer.cornerRadius = 8
        emailTextFieldView.layer.masksToBounds = true
```

이메일 입력하는 텍스트 뷰

```swift
// MARK: - 이메일 입력하는 텍스트 뷰
    private lazy var emailTextFieldView: UIView = {
        let view = UIView()
        view.backgroundColor = #colorLiteral(red: 0.3333333433, green: 0.3333333433, blue: 0.3333333433, alpha: 1)
        view.layer.cornerRadius = 5
        view.clipsToBounds = true
        view.addSubview(emailTextField)
        view.addSubview(emailInfoLabel)
        return view
    }()
```

“이메일 또는 전화번호” 안내문구

```swift
private let emailInfoLabel: UILabel = {
        let label = UILabel()
        label.text = "이메일주소 또는 전화번호"
        label.font = UIFont.systemFont(ofSize: 18)
        label.textColor = #colorLiteral(red: 0.8374180198, green: 0.8374378085, blue: 0.8374271393, alpha: 1)
        return label
    }()
```

로그인 - 이메일 입력 필드

```swift
// 로그인 - 이메일 입력 필드
    private lazy var emailTextField: UITextField = {
        var tf = UITextField()
        tf.frame.size.height = 48
        tf.backgroundColor = .clear
        tf.textColor = .white
        tf.tintColor = .white
        tf.autocapitalizationType = .none
        tf.autocorrectionType = .no
        tf.spellCheckingType = .no
        tf.keyboardType = .emailAddress
        tf.addTarget(self, action: #selector(textFieldEditingChanged(_:)), for: .editingChanged)
        return tf
    }()
```

비밀번호 입력하는 텍스트 뷰

```swift
// MARK: - 비밀번호 입력하는 텍스트 뷰
    private lazy var passwordTextFieldView: UIView = {
        let view = UIView()
        //view.frame.size.height = 48
        view.backgroundColor = #colorLiteral(red: 0.2, green: 0.2, blue: 0.2, alpha: 1)
        view.layer.cornerRadius = 5
        view.clipsToBounds = true
        view.addSubview(passwordTextField)
        view.addSubview(passwordInfoLabel)
        view.addSubview(passwordSecureButton)
        return view
    }()
```

패스워드 텍스트필드의 안내문구

```swift
// 패스워드텍스트필드의 안내문구
    private let passwordInfoLabel: UILabel = {
        let label = UILabel()
        label.text = "비밀번호"
        label.font = UIFont.systemFont(ofSize: 18)
        label.textColor = #colorLiteral(red: 0.8374180198, green: 0.8374378085, blue: 0.8374271393, alpha: 1)
        return label
    }()
```

로그인 - 비밀번호 입력 필드

```swift
// 로그인 - 비밀번호 입력 필드
    private lazy var passwordTextField: UITextField = {
        let tf = UITextField()
        tf.backgroundColor = #colorLiteral(red: 0.2, green: 0.2, blue: 0.2, alpha: 1)
        tf.frame.size.height = 48
        tf.backgroundColor = .clear
        tf.textColor = .white
        tf.tintColor = .white
        tf.autocapitalizationType = .none
        tf.autocorrectionType = .no
        tf.spellCheckingType = .no
        tf.isSecureTextEntry = true
        tf.clearsOnBeginEditing = false
        tf.addTarget(self, action: #selector(textFieldEditingChanged(_:)), for: .editingChanged)
        return tf
    }()
```

패스워드에 “표시”버튼 비밀번호 가리기 기능

```swift
// 패스워드에 "표시"버튼 비밀번호 가리기 기능
    private lazy var passwordSecureButton: UIButton = {
        let button = UIButton(type: .custom)
        button.setTitle("표시", for: .normal)
        button.setTitleColor(#colorLiteral(red: 0.8374180198, green: 0.8374378085, blue: 0.8374271393, alpha: 1), for: .normal)
        button.titleLabel?.font = UIFont.systemFont(ofSize: 14, weight: .light)
        button.addTarget(self, action: #selector(passwordSecureModeSetting), for: .touchUpInside)
        return button
    }()
```

로그인 버튼

```swift
// MARK: - 로그인버튼
    private lazy var loginButton: UIButton = {
        let button = UIButton(type: .custom)
        button.backgroundColor = .clear
        button.layer.cornerRadius = 5
        button.layer.borderWidth = 1
        button.layer.borderColor = #colorLiteral(red: 0.2, green: 0.2, blue: 0.2, alpha: 1)
        button.setTitle("로그인", for: .normal)
        button.titleLabel?.font = UIFont.boldSystemFont(ofSize: 16)
        button.isEnabled = false
        button.addTarget(self, action: #selector(loginButtonTapped), for: .touchUpInside)
        return button
    }()
```

이메일 텍스트필드, 패스워드, 로그인버튼 스택뷰에 배치

```swift
private lazy var stackView: UIStackView = {
        let stview = UIStackView(arrangedSubviews: [emailTextFieldView, passwordTextFieldView, loginButton])
        stview.spacing = 18
        stview.axis = .vertical
        stview.distribution = .fillEqually
        stview.alignment = .fill
        return stview
    }()
```

비밀번호 재설정 버튼

```swift
// 비밀번호 재설정 버튼
    private lazy var passwordResetButton: UIButton = {
        let button = UIButton()
        button.backgroundColor = .clear
        button.setTitle("비밀번호 재설정", for: .normal)
        button.titleLabel?.font = UIFont.boldSystemFont(ofSize: 14)
        button.addTarget(self, action: #selector(resetButtonTapped  ), for: .touchUpInside)
        return button
    }()
```

뷰의 높이 기본값 설정 : 향후 기본 설정 값 변경만으로 관련된 높이 설정 제어/변경 가능

```swift
// 3개의 각 텍스트필드 및 로그인 버튼의 높이 설정
    private let textViewHeight: CGFloat = 48

--------------------------------------------------------------------------------
stackView.heightAnchor.constraint(equalToConstant: textViewHeight*3 + 36)

passwordResetButton.heightAnchor.constraint(equalToConstant: textViewHeight)
```

오토레이아웃 향후 변경을 위한 변수(애니메이션)

```swift
// 오토레이아웃 향후 변경을 위한 변수(애니메이션)
    lazy var emailInfoLabelCenterYConstraint = emailInfoLabel.centerYAnchor.constraint(equalTo: emailTextFieldView.centerYAnchor)
    lazy var passwordInfoLabelCenterYConstraint = passwordInfoLabel.centerYAnchor.constraint(equalTo: passwordTextFieldView.centerYAnchor)
    
    override func viewDidLoad() {
        super.viewDidLoad()
  
    }
```

셋팅(텍스트 필드 델리게이트 설정)

```swift
// 셋팅
    private func configure() {
        view.backgroundColor = #colorLiteral(red: 0.07450980392, green: 0.07450980392, blue: 0.07450980392, alpha: 1)
        emailTextField.delegate = self
        passwordTextField.delegate = self
        [stackView, passwordResetButton].forEach { view.addSubview($0) }
    }
```

비밀번호 가리기 모드 켜고 끄기

```swift
// MARK: - 비밀번호 가리기 모드 켜고 끄기
    @objc private func passwordSecureModeSetting() {
        // 이미 텍스트필드에 내장되어 있는 기능
        passwordTextField.isSecureTextEntry.toggle()
    }
```

로그인 버튼 누르면 동작하는 함수

```swift
@objc func loginButtonTapped() {
        // 서버랑 통신해서, 다음 화면으로 넘어가는 내용 구현
        print("다음 화면으로 넘어가기")
    }
```

리셋버튼이 눌리면 동작하는 함수(alert(얼럿) 창 만들기)

```swift
// 리셋버튼이 눌리면 동작하는 함수
    @objc func resetButtonTapped() {
        //만들기
        let alert = UIAlertController(title: "비밀번호 바꾸기", message: "비밀번호를 바꾸시겠습니까?", preferredStyle: .alert)
        let success = UIAlertAction(title: "확인", style: .default) { action in
            print("확인버튼이 눌렸습니다.")
        }
        let cancel = UIAlertAction(title: "취소", style: .cancel) { action in
            print("취소버튼이 눌렸습니다.")
        }
        
        alert.addAction(success)
        alert.addAction(cancel)
        
        // 실제 띄우기
        self.present(alert, animated: true, completion: nil)
    }
```

앱의 화면을 터치하면 동작하는 함수(화면 터치하면 키보드 내림)

```swift
override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        self.view.endEditing(true)
    }
```

텍스트필드 편집 시작할떄의 설정 - 문구가 위로 올라가면서 크기 작아지고, 오토레이아웃 업데이트

```swift
extension ViewController: UITextFieldDelegate {
    // MARK: - 텍스트필드 편집 시작할때의 설정 - 문구가 위로올라가면서 크기 작아지고, 오토레이아웃 업데이트
    func textFieldDidBeginEditing(_ textField: UITextField) {
        
        if textField == emailTextField {
            emailTextFieldView.backgroundColor = #colorLiteral(red: 0.2972877622, green: 0.2973434925, blue: 0.297280401, alpha: 1)
            emailInfoLabel.font = UIFont.systemFont(ofSize: 11)
            // 오토레이아웃 업데이트
            emailInfoLabelCenterYConstraint.constant = -13
        }
        
        if textField == passwordTextField {
            passwordTextFieldView.backgroundColor = #colorLiteral(red: 0.2972877622, green: 0.2973434925, blue: 0.297280401, alpha: 1)
            passwordInfoLabel.font = UIFont.systemFont(ofSize: 11)
            // 오토레이아웃 업데이트
            passwordInfoLabelCenterYConstraint.constant = -13
        }
        
        // 실제 레이아웃 변경은 애니메이션으로 줄꺼야
        UIView.animate(withDuration: 0.3) {
            self.stackView.layoutIfNeeded()
        }
    }
```

텍스트필드 편집 종료되면 백그라운드 색 변경 (글자가 한개도 입력 안되었을때는 되돌리기)

```swift
// 텍스트필드 편집 종료되면 백그라운드 색 변경 (글자가 한개도 입력 안되었을때는 되돌리기)
    func textFieldDidEndEditing(_ textField: UITextField) {
        
        if textField == emailTextField {
            emailTextFieldView.backgroundColor = #colorLiteral(red: 0.2, green: 0.2, blue: 0.2, alpha: 1)
            // 빈칸이면 원래로 되돌리기
            if emailTextField.text == "" {
                emailInfoLabel.font = UIFont.systemFont(ofSize: 18)
                emailInfoLabelCenterYConstraint.constant = 0
            }
        }
        if textField == passwordTextField {
            passwordTextFieldView.backgroundColor = #colorLiteral(red: 0.2, green: 0.2, blue: 0.2, alpha: 1)
            // 빈칸이면 원래로 되돌리기
            if passwordTextField.text == "" {
                passwordInfoLabel.font = UIFont.systemFont(ofSize: 18)
                passwordInfoLabelCenterYConstraint.constant = 0
            }
        }
        
        // 실제 레이아웃 변경은 애니메이션으로 줄꺼야
        UIView.animate(withDuration: 0.3) {
            self.stackView.layoutIfNeeded()
        }
    }
```

이메일텍스트필드,  비밀번호 텍스트필드 두가지 다 채워져 있을때, 로그인 버튼 빨간색으로 변경

```swift
// MARK: - 이메일텍스트필드, 비밀번호 텍스트필드 두가지 다 채워져 있을때, 로그인 버튼 빨간색으로 변경
    @objc private func textFieldEditingChanged(_ textField: UITextField) {
        if textField.text?.count == 1 {
            if textField.text?.first == " " {
                textField.text = ""
                return
            }
        }
        guard
            let email = emailTextField.text, !email.isEmpty,
            let password = passwordTextField.text, !password.isEmpty
        else {
            loginButton.backgroundColor = .clear
            loginButton.isEnabled = false
            return
        }
        loginButton.backgroundColor = .red
        loginButton.isEnabled = true
    }
```

엔터 누르면 일단 키보드 내림

```swift
// 엔터 누르면 일단 키보드 내림
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        textField.resignFirstResponder()
        return true
    }
}
```

애니메이션 효과

```swift
UIView.animate(withDuration: 0.3) {
            self.stackView.layoutIfNeeded()
```

오토레이아웃 설정을 위한 변수 선언

```swift
//오토레이아웃 향후 변경을 위한 변수(애니메이션)
    lazy var emailInfoLabelCenterYConstraint = emailInfoLabel.centerYAnchor.constraint(equalTo: emailTextFieldView.centerYAnchor)
    
    lazy var passwordInfoLabelCenterYConstraint = passwordInfoLabel.centerYAnchor.constraint(equalTo: passwordTextFieldView.centerYAnchor)
```

코드로 화면 이동하는 방법

```swift
// 1) 코드로 화면 이동 (다음화면이 코드로 작성되어있을때만 가능한 방법)
    @IBAction func codeNextButtonTapped(_ sender: UIButton) {
        let firstVC = FirstViewController()
        
        firstVC.someString = "아기상어"
        
        firstVC.modalPresentationStyle = .fullScreen
        present(firstVC, animated: true)
    }
```

⭐️코드로 스토리보드 객체 생성 후 화면 이동 방법⭐️

```swift
// 2) 코드로 스토리보드 객체를 생성해서, 화면 이동
    @IBAction func storyboardWithCodeButtonTapped(_ sender: UIButton) {

        if let secondVC = storyboard?.instantiateViewController(withIdentifier: "secondVC") as? SecondViewController {
            
            secondVC.modalPresentationStyle = .fullScreen
            secondVC.someString = "아빠상어"
            
            present(secondVC, animated: true)
        }

// 1. 스토리보드 -> 라이브러리-뷰컨트롤러 생성 
// -> swift 파일 생성(코코아, Class이름, UIViewController 선택) 
// -> identity inspector 에서 identity (Storyboard ID 입력) 
// -> 레이블, 버튼 생성 후 @IB 연결
// -> 데이터 전달을 위한 property 생성
// -> 레이블, 버튼 설정
```

스토리보드에서의 화면 이동(간접 세그웨이) (performSegue, prepare는 항상 같이 다님)

```swift
// 3) 스토리보드에서의 화면 이동(간접 세그웨이)
@IBAction func storyboardWithSegueButtonTapped(_ sender: UIButton) {
        

        performSegue(withIdentifier: "toThirdVC", sender: self)
    }

override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        
        if segue.identifier == "toThirdVC" {
            
            let thirdVC = segue.destination as! ThirdViewController
            
            thirdVC.someString = "엄마 상어"
            //데이터 전달
    }

//performSegue(조건 설정) -> override prepare(데이터 전달)
```

이전화면으로 돌아가는 기능

```swift
//programmatically

button.addTarget(self, action: #selector(backButtonTapped), for: .touchUpInside)

-------

@objc func backButtonTapped() {
        //print("뒤로가기 버튼 눌렸음")
        dismiss(animated: true)
    }

//storyboard
@IBAction func backButtonTapped(_ sender: UIButton) {
        dismiss(animated: true)
    }
```

스토리보드에서의 화면 이동(직접 세그웨이)

```swift
var num = 3
    
    override func shouldPerformSegue(withIdentifier identifier: String, sender: Any?) -> Bool {
        if num > 5 {
            return false
        } else {
            return true
        }
    }

//직접 연결이 되었을때 조건에 따라 shouldPerformSegue 메서드의 실행 유무 확인
```

상위 텍스트필드 입력 후 return 버튼 눌렀을 경우 하위 텍스트필드로 넘어가도록 하는 논리

```swift
func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        if heightTextField.text != "", weightTextField.text != "" {
            weightTextField.resignFirstResponder()
            return true
        } else if heightTextField.text != "" {
            weightTextField.becomeFirstResponder()
            return true
        }
        return false
    }
```

일반적인 반올림 코드

```swift
bmi = round(bmi * 10) / 10
```

높이를 넓이 기준으로 1:1로 설정하는 오토레이아웃 코드(multiplier)

```swift
myButton.heightAnchor.constraint(equalTo: myButton.widthAnchor, multiplier: 1).isActive = true
```

버튼을 원 으로 만드는 코드

```swift
self.layer.cornerRadius = self.frame.width / 2
self.myButton.layer.cornerRadius = self.myButton.frame.width / 2
```

탭바/네비게이션 컨트롤러 코드 생성 방법

```swift
@objc func nextButtonTapped() {
        // 탭바컨트롤러의 생성
        let tabBarVC = UITabBarController()
        
        // 첫번째 화면은 네비게이션컨트롤러로 만들기 (기본루트뷰 설정)
        let vc1 = UINavigationController(rootViewController: FirstViewController())
        let vc2 = SecondViewController()
        let vc3 = ThirdViewController()
        let vc4 = FourthViewController()
        let vc5 = FifthViewController()
        
        // 탭바 이름들 설정
        vc1.title = "Main"
        vc2.title = "Search"
        vc3.title = "Post"
        vc4.title = "Likes"
        vc5.title = "Me"
        
        // 탭바로 사용하기 위한 뷰 컨트롤러들 설정
        tabBarVC.setViewControllers([vc1, vc2, vc3, vc4, vc5], animated: false)
        tabBarVC.modalPresentationStyle = .fullScreen
        tabBarVC.tabBar.backgroundColor = .white
        
        // 탭바 이미지 설정 (이미지는 애플이 제공하는 것으로 사용)
        guard let items = tabBarVC.tabBar.items else { return }
        
        items[0].image = UIImage(systemName: "square.and.arrow.up")
        items[1].image = UIImage(systemName: "folder")
        items[2].image = UIImage(systemName: "paperplane")
        items[3].image = UIImage(systemName: "doc")
        items[4].image = UIImage(systemName: "note")
        
        // 프리젠트로 탭바를 띄우기
        present(tabBarVC, animated: true, completion: nil)
    }
```

SceneDelegate → willConnectTo 네비/탭바 설정 후 실행 코드(코드로 생성 시 실행 코드)

```swift
// 기본루트뷰를 탭바컨트롤러로 설정⭐️⭐️⭐️
        window?.rootViewController = tabBarVC
        window?.makeKeyAndVisible()
```

코드 제작 / 네비게이션 바 설정 코드

```swift
let appearance = UINavigationBarAppearance()
        appearance.configureWithOpaqueBackground()  // 불투명으로
        //appearance.backgroundColor = .brown     // 색상설정
        
        //appearance.configureWithTransparentBackground()  // 투명으로
        
        navigationController?.navigationBar.tintColor = .blue
        navigationController?.navigationBar.standardAppearance = appearance
        navigationController?.navigationBar.compactAppearance = appearance
        navigationController?.navigationBar.scrollEdgeAppearance = appearance
```

코드로 네비게이션바 만들때 추가해야하는 코드

```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let windowScene = (scene as? UIWindowScene) else { return }
        window = UIWindow(windowScene: windowScene)

        let naviVC = UINavigationController(rootViewController: ViewController())

        window?.rootViewController = naviVC
        window?.makeKeyAndVisible()
    }
```

피커 사용 설정 (피커뷰 컨트롤러 델리게이트 확장) *피커뷰 = 사진첩에 접근하여 사진을 가져옴 (iOS 14버전 이후 부터 사용 가능)

```swift
extension DetailViewController: PHPickerViewControllerDelegate {
    func picker(_ picker: PHPickerViewController, didFinishPicking results: [PHPickerResult]) {
        picker.dismiss(animated: true)
        
        let itemProvider = results.first?.itemProvider
        
        if let itemProvider = itemProvider, itemProvider.canLoadObject(ofClass: UIImage.self) {
            itemProvider.loadObject(ofClass: UIImage.self) { (image, error) in
                DispatchQueue.main.async {
                    self.detailView.mainImageView.image = image as? UIImage
                }
            }
        } else {
            print("이미지 못 불러왔음!!!!")
        }
    }

}
```

피커뷰 실행 함수

```swift
func setupImagePicker() {
        var configuration = PHPickerConfiguration()
        configuration.selectionLimit = 0 // 0 - 무한대
        configuration.filter = .any(of: [.images, .videos]) // images, videos 선택에 따른 제한
        
        let picker = PHPickerViewController(configuration: configuration)
        picker.delegate = self
        self.present(picker, animated: true)
    }
```

이미지뷰가 눌렸을때의 동작 설정 (피커뷰 실행함수가 포함된) → viewDidLoad() 에 해당 함수 추가 필요

```swift
func setupTapGestures() {
        let tapGesture = UITapGestureRecognizer(target: self, action: #selector(touchUpImageView))
        detailView.mainImageView.addGestureRecognizer(tapGesture)
        detailView.mainImageView.isUserInteractionEnabled = true
    }
    
    @objc func touchUpImageView() {
        print("이미지뷰 터치")
        
        var configuration = PHPickerConfiguration()
        configuration.selectionLimit = 0
        configuration.filter = .any(of: [.images, .videos])
        
        let picker = PHPickerViewController(configuration: configuration)
        picker.delegate = self
        self.present(picker, animated: true)
    }
```

텍스트필드 수정 불가 하도록 설정 (생성자 or  뷰디드로드에 설정 하고자 한 텍스트 필드 → memberIdTextField.delegate = self 필수입력)

```swift
extension DetailView: UITextFieldDelegate {
    func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
         
        if textField == memberIdTextField{
            return false
        }

        return true
    }
}
```

Search Bar 활용 법 

```swift
extension ViewController: UISearchBarDelegate {

    // 유저가 글자를 입력하는 순간마다 호출되는 메서드
		// 글자를 입력 할 때 마다 화면 리로드
    func searchBar(_ searchBar: UISearchBar, textDidChange searchText: String) {

        print(searchText)
        // 다시 빈 배열로 만들기 ⭐️
        self.musicArrays = []

        // 네트워킹 시작
        networkManager.fetchMusic(searchTerm: searchText) { result in
            switch result {
            case .success(let musicDatas):
                self.musicArrays = musicDatas
                DispatchQueue.main.async {
                    self.musicTableView.reloadData()
                }
            case .failure(let error):
                print(error.localizedDescription)
            }
        }
    }

----------------------------------------------------------------------------------

    // 검색(Search) 버튼을 눌렀을때 호출되는 메서드
		// 검색(Search) 버튼을 누르기 전 까지 화면 리로드 하지 않음
    func searchBarSearchButtonClicked(_ searchBar: UISearchBar) {
        guard let text = searchController.searchBar.text else {
            return
        }
        print(text)
        // 다시 빈 배열로 만들기 ⭐️
        self.musicArrays = []

        // 네트워킹 시작
        networkManager.fetchMusic(searchTerm: text) { result in
            switch result {
            case .success(let musicDatas):
                self.musicArrays = musicDatas
                DispatchQueue.main.async {
                    self.musicTableView.reloadData()
                }
            case .failure(let error):
                print(error.localizedDescription)
            }
        }
        self.view.endEditing(true)
    }
}
```

컬렉션 뷰 레이아웃 설정

```swift
func setupCollectionView() {
        collectionView.dataSource = self
        collectionView.backgroundColor = .white
        flowLayout.scrollDirection = .vertical  //스크롤 방향
        
        let collectionCellWidth = 
(UIScreen.main.bounds.width - CVCell.spacingWitch * (CVCell.cellColumns - 1)) / CVCell.cellColumns //셀 간격 설정
        
        flowLayout.itemSize = CGSize(width: collectionCellWidth, height: collectionCellWidth)
        flowLayout.minimumLineSpacing = CVCell.spacingWitch
        flowLayout.minimumLineSpacing = CVCell.spacingWitch
        
        collectionView.collectionViewLayout = flowLayout
    }
```

URL ⇒ 이미지를 세팅하는 메서드

```swift
private func loadImage() {
        guard let urlString = self.imageUrl, let url = URL(string: urlString) else { return }
        DispatchQueue.global().async {
            guard let data = try? Data(contentsOf: url) else { return }
            // 오래걸리는 작업이 일어나고 있는 동안에 url이 바뀔 가능성 제거 ⭐️⭐️⭐️
            guard urlString == url.absoluteString else { return }
            
            DispatchQueue.main.async {
                self.mainImageView.image = UIImage(data: data)
            }
        }
    }
}

//ViewController
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

let cell = musicTableView.dequeueReusableCell(withIdentifier: Cell.musicCellIdentifier, for: indexPath) as! MusicCell
        
        cell.imageUrl = musicArray[indexPath.row].imageUrl
        cell.songNameLabel.text = musicArray[indexPath.row].songName
        cell.artistNameLabel.text = musicArray[indexPath.row].artistName
        cell.albumNameLabel.text = musicArray[indexPath.row].albumName
        cell.releaseDateLabel.text = musicArray[indexPath.row].releaseDateString
        
        cell.selectionStyle = .none
        
        return cell
}
```

텍스트 뷰에서 플레이스홀더를 만드는 방법

```swift
extension DetailViewController: UITextViewDelegate {
    // 입력을 시작할때
    // (텍스트뷰는 플레이스홀더가 따로 있지 않아서, 플레이스 홀더처럼 동작하도록 직접 구현)
    func textViewDidBeginEditing(_ textView: UITextView) {
        if textView.text == "텍스트를 여기에 입력하세요." {
            textView.text = nil
            textView.textColor = .black
        }
    }
    
    // 입력이 끝났을때
    func textViewDidEndEditing(_ textView: UITextView) {
        // 비어있으면 다시 플레이스 홀더처럼 입력하기 위해서 조건 확인
        if textView.text.trimmingCharacters(in: .whitespacesAndNewlines).isEmpty {
            textView.text = "텍스트를 여기에 입력하세요."
            textView.textColor = .lightGray
        }
    }
}
```

Hex Color (16진수로 표현된 컬러) 적용 UIColor  확장 설정 방법

```swift
import UIKit

extension UIColor {

    // Hex String -> UIColor
    convenience init(hexString: String) {
        let hexString = hexString.trimmingCharacters(in: .whitespacesAndNewlines)
        let scanner = Scanner(string: hexString)
         
        if hexString.hasPrefix("#") {
            scanner.currentIndex = scanner.string.index(after: scanner.currentIndex)
        }
         
        var color: UInt64 = 0
        scanner.scanHexInt64(&color)
         
        let mask = 0x000000FF
        let r = Int(color >> 16) & mask
        let g = Int(color >> 8) & mask
        let b = Int(color) & mask
         
        let red   = CGFloat(r) / 255.0
        let green = CGFloat(g) / 255.0
        let blue  = CGFloat(b) / 255.0
         
        self.init(red: red, green: green, blue: blue, alpha: 1)
    }
     
    // UIColor -> Hex String
    var hexString: String? {
        var red: CGFloat = 0
        var green: CGFloat = 0
        var blue: CGFloat = 0
        var alpha: CGFloat = 0
         
        let multiplier = CGFloat(255.999999)
         
        guard self.getRed(&red, green: &green, blue: &blue, alpha: &alpha) else {
            return nil
        }
         
        if alpha == 1.0 {
            return String(
                format: "#%02lX%02lX%02lX",
                Int(red * multiplier),
                Int(green * multiplier),
                Int(blue * multiplier)
            )
        }
        else {
            return String(
                format: "#%02lX%02lX%02lX%02lX",
                Int(red * multiplier),
                Int(green * multiplier),
                Int(blue * multiplier),
                Int(alpha * multiplier)
            )
        }
    }
}
```

테이블뷰의 높이 자동 적용

```swift
// 테이블뷰의 높이를 자동적으로 추청하도록 하는 메서드
    // (ToDo에서 메세지가 길때는 셀의 높이를 더 높게 ==> 셀의 오토레이아웃 설정도 필요)
    func tableView(_ tableView: UITableView, estimatedHeightForRowAt indexPath: IndexPath) -> CGFloat {
        return UITableView.automaticDimension
    }
}
```

Request 실행 함수 (비동기적 실행 ==> 클로저 방식으로 끝난 시점을 전달 받도록 설계)

```swift
private func performRequest(with urlString: String, completion: @escaping (Result<[Music], NetworkError>) -> Void) {
        //print(#function)
        guard let url = URL(string: urlString) else { return }
        
        let session = URLSession(configuration: .default)
        
        let task = session.dataTask(with: url) { (data, response, error) in
            if error != nil {
                print(error!)
                completion(.failure(.networkingError))
                return
            }
            guard let safeData = data else {
                completion(.failure(.dataError))
                return
            }
            
            // 데이터 분석하기
            if let musics = self.parseJSON(safeData) {
                print("Parse 실행")
                completion(.success(musics))
            } else {
                print("Parse 실패")
                completion(.failure(.parseError))
            }
        }
        task.resume()
    }
```

JSON 파일을 parsing하는 방법 (JSONDecoder의 활용)

```swift
private func parseJSON(_ musicData: Data) -> [Music]? {
        //print(#function)
        // 성공
        do {
            // 우리가 만들어 놓은 구조체(클래스 등)로 변환하는 객체와 메서드
            // (JSON 데이터 ====> MusicData 클래스)
            let musicData = try JSONDecoder().decode(MusicData.self, from: musicData)
            return musicData.results
        // 실패
        } catch {
            print(error.localizedDescription)
            return nil
        }
    }
```
