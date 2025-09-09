# iOS Development Cheat Sheet

## Quick Reference

iOS development involves creating apps for iPhone, iPad, and other Apple devices using Swift/Objective-C, Xcode, and Apple's frameworks.

### Setup and Installation

```bash
# Install Xcode from Mac App Store or Apple Developer

# Install Xcode Command Line Tools
xcode-select --install

# Verify installation
xcode-select -p
swift --version

# Install CocoaPods (dependency manager)
sudo gem install cocoapods

# Install Carthage (alternative dependency manager)
brew install carthage

# Install Swift Package Manager (built into Xcode)
# No separate installation needed
```

## Swift Fundamentals

### Variables and Constants

```swift
// Constants (immutable)
let name = "John"
let age = 30
let pi: Double = 3.14159

// Variables (mutable)
var score = 0
var isActive = true
var message: String? = nil

// Type inference
let number = 42          // Int
let price = 19.99        // Double
let isValid = true       // Bool

// Explicit typing
let username: String = "user123"
let count: Int = 5
```

### Basic Types

```swift
// Numbers
let integer: Int = 42
let float: Float = 3.14
let double: Double = 3.14159
let decimal = NSDecimalNumber(string: "123.45")

// Strings
let message = "Hello, World!"
let multiline = """
    This is a
    multiline string
    """

// String interpolation
let greeting = "Hello, \(name). You are \(age) years old."

// Booleans
let isEnabled: Bool = true
let isCompleted = false

// Arrays
var numbers: [Int] = [1, 2, 3, 4, 5]
var names = ["Alice", "Bob", "Charlie"]
numbers.append(6)
names.insert("David", at: 0)

// Dictionaries
var person: [String: Any] = [
    "name": "John",
    "age": 30,
    "isActive": true
]
person["email"] = "john@example.com"

// Sets
var uniqueNumbers: Set<Int> = [1, 2, 3, 4, 5]
uniqueNumbers.insert(6)

// Tuples
let coordinates = (x: 10.0, y: 20.0)
let httpResponse = (statusCode: 200, message: "OK")
```

### Functions

```swift
// Basic function
func greet(name: String) -> String {
    return "Hello, \(name)!"
}

// Function with multiple parameters
func calculateArea(width: Double, height: Double) -> Double {
    return width * height
}

// Function with default parameters
func sayHello(to name: String = "World") -> String {
    return "Hello, \(name)!"
}

// Function with parameter labels
func move(from start: Int, to end: Int) {
    print("Moving from \(start) to \(end)")
}
// Usage: move(from: 1, to: 5)

// Variadic parameters
func sum(_ numbers: Int...) -> Int {
    return numbers.reduce(0, +)
}

// Higher-order functions
func processNumbers(_ numbers: [Int], operation: (Int) -> Int) -> [Int] {
    return numbers.map(operation)
}

let doubled = processNumbers([1, 2, 3]) { $0 * 2 }

// Closures
let multiply = { (a: Int, b: Int) -> Int in
    return a * b
}

// Simplified closure syntax
let numbers = [1, 2, 3, 4, 5]
let squared = numbers.map { $0 * $0 }
let filtered = numbers.filter { $0 > 2 }
```

### Classes and Structures

```swift
// Structure
struct Point {
    var x: Double
    var y: Double
    
    // Computed property
    var magnitude: Double {
        return sqrt(x * x + y * y)
    }
    
    // Methods
    func distance(to other: Point) -> Double {
        let dx = x - other.x
        let dy = y - other.y
        return sqrt(dx * dx + dy * dy)
    }
    
    // Mutating method (for structs)
    mutating func moveBy(dx: Double, dy: Double) {
        x += dx
        y += dy
    }
}

// Class
class Person {
    var name: String
    var age: Int
    
    // Initializer
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    
    // Computed property
    var description: String {
        return "\(name) is \(age) years old"
    }
    
    // Method
    func celebrate() {
        age += 1
        print("Happy birthday! Now \(age)")
    }
}

// Inheritance
class Student: Person {
    var school: String
    
    init(name: String, age: Int, school: String) {
        self.school = school
        super.init(name: name, age: age)
    }
    
    override var description: String {
        return super.description + " and studies at \(school)"
    }
}

// Protocol
protocol Drawable {
    func draw()
}

extension Point: Drawable {
    func draw() {
        print("Drawing point at (\(x), \(y))")
    }
}
```

## UIKit Framework

### View Controllers

```swift
import UIKit

class ViewController: UIViewController {
    
    @IBOutlet weak var titleLabel: UILabel!
    @IBOutlet weak var textField: UITextField!
    @IBOutlet weak var button: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        setupUI()
    }
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        // Called before view appears
    }
    
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        // Called after view appears
    }
    
    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        // Called before view disappears
    }
    
    override func viewDidDisappear(_ animated: Bool) {
        super.viewDidDisappear(animated)
        // Called after view disappears
    }
    
    private func setupUI() {
        titleLabel.text = "Welcome"
        titleLabel.font = UIFont.systemFont(ofSize: 24, weight: .bold)
        titleLabel.textColor = .systemBlue
        
        textField.placeholder = "Enter your name"
        textField.borderStyle = .roundedRect
        textField.delegate = self
        
        button.setTitle("Submit", for: .normal)
        button.backgroundColor = .systemBlue
        button.layer.cornerRadius = 8
    }
    
    @IBAction func buttonTapped(_ sender: UIButton) {
        guard let text = textField.text, !text.isEmpty else {
            showAlert(message: "Please enter your name")
            return
        }
        titleLabel.text = "Hello, \(text)!"
    }
    
    private func showAlert(message: String) {
        let alert = UIAlertController(title: "Alert", message: message, preferredStyle: .alert)
        alert.addAction(UIAlertAction(title: "OK", style: .default))
        present(alert, animated: true)
    }
}

extension ViewController: UITextFieldDelegate {
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        textField.resignFirstResponder()
        return true
    }
}
```

### UI Components

```swift
// Labels
let label = UILabel()
label.text = "Hello, World!"
label.font = UIFont.systemFont(ofSize: 16)
label.textColor = .black
label.textAlignment = .center
label.numberOfLines = 0 // Multiple lines

// Buttons
let button = UIButton(type: .system)
button.setTitle("Tap me", for: .normal)
button.setTitleColor(.white, for: .normal)
button.backgroundColor = .systemBlue
button.layer.cornerRadius = 8
button.addTarget(self, action: #selector(buttonTapped), for: .touchUpInside)

// Text Fields
let textField = UITextField()
textField.placeholder = "Enter text"
textField.borderStyle = .roundedRect
textField.keyboardType = .emailAddress
textField.isSecureTextEntry = false

// Text View
let textView = UITextView()
textView.text = "Editable text"
textView.font = UIFont.systemFont(ofSize: 16)
textView.isEditable = true
textView.layer.borderColor = UIColor.gray.cgColor
textView.layer.borderWidth = 1

// Image View
let imageView = UIImageView()
imageView.image = UIImage(named: "image.png")
imageView.contentMode = .scaleAspectFit
imageView.clipsToBounds = true

// Switch
let toggle = UISwitch()
toggle.isOn = true
toggle.addTarget(self, action: #selector(switchToggled), for: .valueChanged)

// Slider
let slider = UISlider()
slider.minimumValue = 0
slider.maximumValue = 100
slider.value = 50
slider.addTarget(self, action: #selector(sliderChanged), for: .valueChanged)

// Progress View
let progressView = UIProgressView(progressViewStyle: .default)
progressView.progress = 0.5

// Activity Indicator
let activityIndicator = UIActivityIndicatorView(style: .medium)
activityIndicator.startAnimating()

// Segmented Control
let segmentedControl = UISegmentedControl(items: ["First", "Second", "Third"])
segmentedControl.selectedSegmentIndex = 0
segmentedControl.addTarget(self, action: #selector(segmentChanged), for: .valueChanged)
```

### Table Views

```swift
class TableViewController: UIViewController {
    @IBOutlet weak var tableView: UITableView!
    
    var data = ["Item 1", "Item 2", "Item 3", "Item 4", "Item 5"]
    
    override func viewDidLoad() {
        super.viewDidLoad()
        tableView.dataSource = self
        tableView.delegate = self
        tableView.register(UITableViewCell.self, forCellReuseIdentifier: "Cell")
    }
}

extension TableViewController: UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return data.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
        cell.textLabel?.text = data[indexPath.row]
        cell.accessoryType = .disclosureIndicator
        return cell
    }
    
    func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCell.EditingStyle, forRowAt indexPath: IndexPath) {
        if editingStyle == .delete {
            data.remove(at: indexPath.row)
            tableView.deleteRows(at: [indexPath], with: .fade)
        }
    }
}

extension TableViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        tableView.deselectRow(at: indexPath, animated: true)
        print("Selected: \(data[indexPath.row])")
    }
    
    func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        return 60
    }
}

// Custom Table View Cell
class CustomTableViewCell: UITableViewCell {
    @IBOutlet weak var titleLabel: UILabel!
    @IBOutlet weak var subtitleLabel: UILabel!
    @IBOutlet weak var customImageView: UIImageView!
    
    func configure(with item: DataItem) {
        titleLabel.text = item.title
        subtitleLabel.text = item.subtitle
        customImageView.image = UIImage(named: item.imageName)
    }
}
```

### Collection Views

```swift
class CollectionViewController: UIViewController {
    @IBOutlet weak var collectionView: UICollectionView!
    
    let data = Array(1...50)
    
    override func viewDidLoad() {
        super.viewDidLoad()
        setupCollectionView()
    }
    
    private func setupCollectionView() {
        let layout = UICollectionViewFlowLayout()
        layout.itemSize = CGSize(width: 100, height: 100)
        layout.minimumInteritemSpacing = 10
        layout.minimumLineSpacing = 10
        layout.sectionInset = UIEdgeInsets(top: 20, left: 20, bottom: 20, right: 20)
        
        collectionView.collectionViewLayout = layout
        collectionView.dataSource = self
        collectionView.delegate = self
        collectionView.register(UICollectionViewCell.self, forCellWithReuseIdentifier: "Cell")
    }
}

extension CollectionViewController: UICollectionViewDataSource {
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return data.count
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "Cell", for: indexPath)
        cell.backgroundColor = .systemBlue
        cell.layer.cornerRadius = 8
        return cell
    }
}

extension CollectionViewController: UICollectionViewDelegate {
    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        print("Selected item \(data[indexPath.item])")
    }
}
```

### Navigation

```swift
// Navigation Controller
let navController = UINavigationController(rootViewController: viewController)

// Push/Pop view controllers
navigationController?.pushViewController(nextViewController, animated: true)
navigationController?.popViewController(animated: true)
navigationController?.popToRootViewController(animated: true)

// Present/Dismiss modally
present(modalViewController, animated: true) {
    // Completion block
}
dismiss(animated: true) {
    // Completion block
}

// Segues (in Storyboard)
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    if segue.identifier == "ShowDetail" {
        if let detailVC = segue.destination as? DetailViewController {
            detailVC.data = selectedData
        }
    }
}

// Programmatic navigation
@IBAction func navigateToDetail(_ sender: Any) {
    let storyboard = UIStoryboard(name: "Main", bundle: nil)
    if let detailVC = storyboard.instantiateViewController(withIdentifier: "DetailViewController") as? DetailViewController {
        detailVC.data = selectedData
        navigationController?.pushViewController(detailVC, animated: true)
    }
}

// Tab Bar Controller
let tabBarController = UITabBarController()
tabBarController.viewControllers = [
    UINavigationController(rootViewController: firstViewController),
    UINavigationController(rootViewController: secondViewController),
    UINavigationController(rootViewController: thirdViewController)
]
```

## SwiftUI Framework

### Basic SwiftUI Views

```swift
import SwiftUI

struct ContentView: View {
    @State private var name = ""
    @State private var count = 0
    @State private var isToggled = false
    
    var body: some View {
        NavigationView {
            VStack(spacing: 20) {
                Text("Hello, SwiftUI!")
                    .font(.largeTitle)
                    .fontWeight(.bold)
                    .foregroundColor(.blue)
                
                TextField("Enter your name", text: $name)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
                    .padding(.horizontal)
                
                HStack {
                    Button("Decrement") {
                        count -= 1
                    }
                    .buttonStyle(BorderedButtonStyle())
                    
                    Text("\(count)")
                        .font(.title)
                        .frame(minWidth: 50)
                    
                    Button("Increment") {
                        count += 1
                    }
                    .buttonStyle(BorderedButtonStyle())
                }
                
                Toggle("Enable Feature", isOn: $isToggled)
                    .padding(.horizontal)
                
                if !name.isEmpty {
                    Text("Welcome, \(name)!")
                        .font(.headline)
                        .transition(.opacity)
                }
                
                Spacer()
            }
            .navigationTitle("SwiftUI Demo")
            .padding()
        }
    }
}

// Preview
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

### SwiftUI Components

```swift
// Text
Text("Hello, World!")
    .font(.title)
    .fontWeight(.bold)
    .foregroundColor(.primary)
    .multilineTextAlignment(.center)
    .lineLimit(nil)

// Button
Button(action: {
    // Action
}) {
    HStack {
        Image(systemName: "star.fill")
        Text("Favorite")
    }
}
.buttonStyle(BorderedProminentButtonStyle())

// Image
Image("image-name")
    .resizable()
    .aspectRatio(contentMode: .fit)
    .frame(width: 200, height: 200)
    .clipShape(Circle())

Image(systemName: "heart.fill")
    .foregroundColor(.red)
    .font(.system(size: 24))

// List
List {
    ForEach(items, id: \.id) { item in
        HStack {
            Text(item.name)
            Spacer()
            Text(item.date, style: .date)
        }
    }
    .onDelete(perform: deleteItems)
}

// Form
Form {
    Section(header: Text("Personal Information")) {
        TextField("Name", text: $name)
        TextField("Email", text: $email)
        DatePicker("Birthday", selection: $birthday, displayedComponents: .date)
    }
    
    Section(header: Text("Preferences")) {
        Toggle("Notifications", isOn: $notificationsEnabled)
        Picker("Theme", selection: $selectedTheme) {
            ForEach(themes, id: \.self) { theme in
                Text(theme)
            }
        }
        .pickerStyle(SegmentedPickerStyle())
    }
}

// Navigation
NavigationView {
    List(items, id: \.id) { item in
        NavigationLink(destination: DetailView(item: item)) {
            Text(item.name)
        }
    }
    .navigationTitle("Items")
}

// Sheet/Modal
.sheet(isPresented: $showingSheet) {
    SheetView()
}

// Alert
.alert("Alert Title", isPresented: $showingAlert) {
    Button("OK") { }
} message: {
    Text("Alert message")
}
```

### State Management in SwiftUI

```swift
// @State - Local state
struct CounterView: View {
    @State private var count = 0
    
    var body: some View {
        VStack {
            Text("Count: \(count)")
            Button("Increment") {
                count += 1
            }
        }
    }
}

// @Binding - Two-way binding
struct ChildView: View {
    @Binding var text: String
    
    var body: some View {
        TextField("Enter text", text: $text)
    }
}

struct ParentView: View {
    @State private var inputText = ""
    
    var body: some View {
        VStack {
            Text("You typed: \(inputText)")
            ChildView(text: $inputText)
        }
    }
}

// @ObservableObject - External data model
class UserData: ObservableObject {
    @Published var name = ""
    @Published var age = 0
    @Published var isLoggedIn = false
    
    func login() {
        isLoggedIn = true
    }
    
    func logout() {
        isLoggedIn = false
        name = ""
        age = 0
    }
}

struct UserView: View {
    @ObservedObject var userData = UserData()
    
    var body: some View {
        VStack {
            if userData.isLoggedIn {
                Text("Welcome, \(userData.name)!")
                Button("Logout") {
                    userData.logout()
                }
            } else {
                TextField("Name", text: $userData.name)
                Button("Login") {
                    userData.login()
                }
            }
        }
    }
}

// @EnvironmentObject - Shared across views
struct RootView: View {
    let userData = UserData()
    
    var body: some View {
        ContentView()
            .environmentObject(userData)
    }
}

struct ContentView: View {
    @EnvironmentObject var userData: UserData
    
    var body: some View {
        // Use userData here
        Text("Hello, \(userData.name)")
    }
}
```

## Auto Layout and Constraints

### Programmatic Auto Layout

```swift
view.translatesAutoresizingMaskIntoConstraints = false

// Anchor-based constraints
view.leadingAnchor.constraint(equalTo: superview.leadingAnchor, constant: 16).isActive = true
view.trailingAnchor.constraint(equalTo: superview.trailingAnchor, constant: -16).isActive = true
view.topAnchor.constraint(equalTo: superview.safeAreaLayoutGuide.topAnchor, constant: 20).isActive = true
view.heightAnchor.constraint(equalToConstant: 50).isActive = true

// NSLayoutConstraint
NSLayoutConstraint.activate([
    titleLabel.centerXAnchor.constraint(equalTo: view.centerXAnchor),
    titleLabel.centerYAnchor.constraint(equalTo: view.centerYAnchor),
    titleLabel.leadingAnchor.constraint(greaterThanOrEqualTo: view.leadingAnchor, constant: 20),
    titleLabel.trailingAnchor.constraint(lessThanOrEqualTo: view.trailingAnchor, constant: -20)
])

// Stack Views
let stackView = UIStackView(arrangedSubviews: [label1, label2, button])
stackView.axis = .vertical
stackView.distribution = .equalSpacing
stackView.alignment = .center
stackView.spacing = 16

view.addSubview(stackView)
stackView.translatesAutoresizingMaskIntoConstraints = false
NSLayoutConstraint.activate([
    stackView.centerXAnchor.constraint(equalTo: view.centerXAnchor),
    stackView.centerYAnchor.constraint(equalTo: view.centerYAnchor),
    stackView.leadingAnchor.constraint(greaterThanOrEqualTo: view.leadingAnchor, constant: 20),
    stackView.trailingAnchor.constraint(lessThanOrEqualTo: view.trailingAnchor, constant: -20)
])
```

### Visual Format Language

```swift
let views = ["button": button, "label": label]
let metrics = ["spacing": 16, "height": 50]

let horizontalConstraints = NSLayoutConstraint.constraints(
    withVisualFormat: "H:|-16-[button]-spacing-[label]-16-|",
    options: [],
    metrics: metrics,
    views: views
)

let verticalConstraints = NSLayoutConstraint.constraints(
    withVisualFormat: "V:[button(height)]",
    options: [],
    metrics: metrics,
    views: views
)

NSLayoutConstraint.activate(horizontalConstraints + verticalConstraints)
```

## Networking and Data

### URLSession

```swift
import Foundation

// Simple GET request
func fetchData() {
    guard let url = URL(string: "https://api.example.com/data") else { return }
    
    URLSession.shared.dataTask(with: url) { data, response, error in
        if let error = error {
            print("Error: \(error.localizedDescription)")
            return
        }
        
        guard let data = data else { return }
        
        do {
            let result = try JSONDecoder().decode(APIResponse.self, from: data)
            DispatchQueue.main.async {
                // Update UI
            }
        } catch {
            print("Decoding error: \(error)")
        }
    }.resume()
}

// POST request with JSON
func postData<T: Codable>(data: T, to url: URL, completion: @escaping (Result<Data, Error>) -> Void) {
    var request = URLRequest(url: url)
    request.httpMethod = "POST"
    request.setValue("application/json", forHTTPHeaderField: "Content-Type")
    
    do {
        request.httpBody = try JSONEncoder().encode(data)
    } catch {
        completion(.failure(error))
        return
    }
    
    URLSession.shared.dataTask(with: request) { data, response, error in
        if let error = error {
            completion(.failure(error))
            return
        }
        
        guard let data = data else {
            completion(.failure(NetworkError.noData))
            return
        }
        
        completion(.success(data))
    }.resume()
}

// Custom error types
enum NetworkError: Error {
    case noData
    case invalidURL
    case decodingError
}

// Generic API service
class APIService {
    static let shared = APIService()
    private init() {}
    
    func fetch<T: Codable>(_ type: T.Type, from url: URL, completion: @escaping (Result<T, Error>) -> Void) {
        URLSession.shared.dataTask(with: url) { data, response, error in
            if let error = error {
                completion(.failure(error))
                return
            }
            
            guard let data = data else {
                completion(.failure(NetworkError.noData))
                return
            }
            
            do {
                let result = try JSONDecoder().decode(type, from: data)
                completion(.success(result))
            } catch {
                completion(.failure(error))
            }
        }.resume()
    }
}
```

### Core Data

```swift
import CoreData

// Core Data Stack
class CoreDataManager {
    static let shared = CoreDataManager()
    
    lazy var persistentContainer: NSPersistentContainer = {
        let container = NSPersistentContainer(name: "DataModel")
        container.loadPersistentStores { _, error in
            if let error = error {
                fatalError("Core Data error: \(error)")
            }
        }
        return container
    }()
    
    var context: NSManagedObjectContext {
        return persistentContainer.viewContext
    }
    
    func saveContext() {
        let context = persistentContainer.viewContext
        
        if context.hasChanges {
            do {
                try context.save()
            } catch {
                print("Save error: \(error)")
            }
        }
    }
}

// CRUD operations
extension CoreDataManager {
    func createUser(name: String, email: String) -> User? {
        let user = User(context: context)
        user.name = name
        user.email = email
        user.createdAt = Date()
        
        saveContext()
        return user
    }
    
    func fetchUsers() -> [User] {
        let request: NSFetchRequest<User> = User.fetchRequest()
        request.sortDescriptors = [NSSortDescriptor(key: "name", ascending: true)]
        
        do {
            return try context.fetch(request)
        } catch {
            print("Fetch error: \(error)")
            return []
        }
    }
    
    func deleteUser(_ user: User) {
        context.delete(user)
        saveContext()
    }
}
```

### UserDefaults

```swift
extension UserDefaults {
    enum Keys {
        static let username = "username"
        static let isFirstLaunch = "isFirstLaunch"
        static let settings = "userSettings"
    }
}

// Storing data
UserDefaults.standard.set("john_doe", forKey: UserDefaults.Keys.username)
UserDefaults.standard.set(false, forKey: UserDefaults.Keys.isFirstLaunch)

// Storing custom objects
struct Settings: Codable {
    let theme: String
    let notificationsEnabled: Bool
}

let settings = Settings(theme: "dark", notificationsEnabled: true)
if let encoded = try? JSONEncoder().encode(settings) {
    UserDefaults.standard.set(encoded, forKey: UserDefaults.Keys.settings)
}

// Retrieving data
let username = UserDefaults.standard.string(forKey: UserDefaults.Keys.username)
let isFirstLaunch = UserDefaults.standard.bool(forKey: UserDefaults.Keys.isFirstLaunch)

// Retrieving custom objects
if let savedSettings = UserDefaults.standard.object(forKey: UserDefaults.Keys.settings) as? Data {
    if let loadedSettings = try? JSONDecoder().decode(Settings.self, from: savedSettings) {
        // Use loadedSettings
    }
}
```

## Testing

### Unit Testing with XCTest

```swift
import XCTest
@testable import MyApp

class CalculatorTests: XCTestCase {
    var calculator: Calculator!
    
    override func setUpWithError() throws {
        try super.setUpWithError()
        calculator = Calculator()
    }
    
    override func tearDownWithError() throws {
        calculator = nil
        try super.tearDownWithError()
    }
    
    func testAddition() {
        // Given
        let a = 5
        let b = 3
        
        // When
        let result = calculator.add(a, b)
        
        // Then
        XCTAssertEqual(result, 8, "Addition should return correct sum")
    }
    
    func testDivisionByZero() {
        // When & Then
        XCTAssertThrowsError(try calculator.divide(10, by: 0)) { error in
            XCTAssertEqual(error as? CalculatorError, CalculatorError.divisionByZero)
        }
    }
    
    func testAsyncOperation() {
        let expectation = XCTestExpectation(description: "Async operation completes")
        
        calculator.performAsyncOperation { result in
            XCTAssertEqual(result, "success")
            expectation.fulfill()
        }
        
        wait(for: [expectation], timeout: 5.0)
    }
}
```

### UI Testing

```swift
import XCTest

class MyAppUITests: XCTestCase {
    var app: XCUIApplication!
    
    override func setUpWithError() throws {
        continueAfterFailure = false
        app = XCUIApplication()
        app.launch()
    }
    
    func testLoginFlow() {
        // Test elements exist
        let usernameField = app.textFields["usernameField"]
        let passwordField = app.secureTextFields["passwordField"]
        let loginButton = app.buttons["loginButton"]
        
        XCTAssertTrue(usernameField.exists)
        XCTAssertTrue(passwordField.exists)
        XCTAssertTrue(loginButton.exists)
        
        // Interact with elements
        usernameField.tap()
        usernameField.typeText("testuser")
        
        passwordField.tap()
        passwordField.typeText("password")
        
        loginButton.tap()
        
        // Verify result
        let welcomeLabel = app.staticTexts["welcomeLabel"]
        XCTAssertTrue(welcomeLabel.waitForExistence(timeout: 5))
        XCTAssertEqual(welcomeLabel.label, "Welcome, testuser!")
    }
    
    func testTableViewInteraction() {
        let table = app.tables["itemsTable"]
        XCTAssertTrue(table.exists)
        
        let firstCell = table.cells.element(boundBy: 0)
        XCTAssertTrue(firstCell.exists)
        
        firstCell.tap()
        
        // Verify navigation
        let detailView = app.otherElements["detailView"]
        XCTAssertTrue(detailView.waitForExistence(timeout: 5))
    }
}
```

## App Store Deployment

### Build Configuration

```swift
// Build Settings
#if DEBUG
    let baseURL = "https://api-dev.example.com"
    let logLevel = "verbose"
#else
    let baseURL = "https://api.example.com"
    let logLevel = "error"
#endif

// Conditional compilation
#if targetEnvironment(simulator)
    // Simulator specific code
#endif

#if os(iOS)
    // iOS specific code
#elseif os(macOS)
    // macOS specific code
#endif
```

### App Store Connect

```swift
// Info.plist configuration
<key>CFBundleDisplayName</key>
<string>My App</string>
<key>CFBundleVersion</key>
<string>1.0.0</string>
<key>CFBundleShortVersionString</key>
<string>1.0</string>

// Privacy usage descriptions
<key>NSCameraUsageDescription</key>
<string>This app uses camera to take photos</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app uses location to provide relevant content</string>
<key>NSContactsUsageDescription</key>
<string>This app accesses contacts to...</string>
```

### Code Signing and Provisioning

```bash
# Automatic signing in Xcode
# Project Settings -> Signing & Capabilities -> Automatically manage signing

# Manual signing
# 1. Create certificates in Apple Developer Portal
# 2. Create App IDs
# 3. Create provisioning profiles
# 4. Download and install profiles in Xcode
```

### Archive and Upload

```bash
# Command line build
xcodebuild -workspace MyApp.xcworkspace \
           -scheme MyApp \
           -configuration Release \
           -archivePath MyApp.xcarchive \
           archive

# Upload to App Store Connect
xcodebuild -exportArchive \
           -archivePath MyApp.xcarchive \
           -exportPath . \
           -exportOptionsPlist ExportOptions.plist

# Or use Xcode GUI:
# Product -> Archive -> Distribute App
```

## Performance Optimization

### Memory Management

```swift
// ARC (Automatic Reference Counting)
class Person {
    let name: String
    var apartment: Apartment?
    
    init(name: String) {
        self.name = name
    }
    
    deinit {
        print("\(name) is being deinitialized")
    }
}

// Weak references to avoid retain cycles
class Apartment {
    let unit: String
    weak var tenant: Person?
    
    init(unit: String) {
        self.unit = unit
    }
    
    deinit {
        print("Apartment \(unit) is being deinitialized")
    }
}

// Unowned references
class CreditCard {
    let number: UInt64
    unowned let customer: Customer
    
    init(number: UInt64, customer: Customer) {
        self.number = number
        self.customer = customer
    }
}

// Closures and retain cycles
class HTTPClient {
    var onCompletion: (() -> Void)?
    
    func performRequest() {
        // Weak self to avoid retain cycle
        onCompletion = { [weak self] in
            self?.handleCompletion()
        }
    }
    
    private func handleCompletion() {
        // Handle completion
    }
}
```

### Performance Best Practices

```swift
// Lazy properties
class DataManager {
    lazy var expensiveResource: ExpensiveResource = {
        return ExpensiveResource()
    }()
    
    // Only computed when accessed
    var formattedDate: String {
        return dateFormatter.string(from: Date())
    }
}

// Efficient table view cell reuse
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath) as! CustomCell
    
    // Configure cell
    cell.configure(with: data[indexPath.row])
    
    return cell
}

// Image caching
class ImageCache {
    static let shared = ImageCache()
    private let cache = NSCache<NSString, UIImage>()
    
    func setImage(_ image: UIImage, forKey key: String) {
        cache.setObject(image, forKey: key as NSString)
    }
    
    func getImage(forKey key: String) -> UIImage? {
        return cache.object(forKey: key as NSString)
    }
}

// Background processing
DispatchQueue.global(qos: .background).async {
    // Heavy computation
    let result = performHeavyComputation()
    
    DispatchQueue.main.async {
        // Update UI
        self.updateUI(with: result)
    }
}
```

## Debugging with Xcode

### Debugging Tools

```swift
// Breakpoints
// Set breakpoints by clicking line numbers in Xcode

// Conditional breakpoints
// Right-click breakpoint -> Edit Breakpoint -> Condition

// Print debugging
print("Debug value: \(variable)")
debugPrint("Debug info: \(complexObject)")

// Assertions
assert(condition, "Error message")
assertionFailure("This should never happen")

// Fatal errors
fatalError("Critical error occurred")
```

### Instruments

```bash
# Profiling tools in Xcode
# Product -> Profile -> Choose template:
# - Time Profiler: CPU usage
# - Allocations: Memory usage
# - Leaks: Memory leaks
# - Energy Log: Battery usage
# - Network: Network activity
```

### LLDB Console

```bash
# Print variables
(lldb) po variableName
(lldb) p variableName

# Set values
(lldb) expr variableName = newValue

# Continue execution
(lldb) continue
(lldb) c

# Step execution
(lldb) step
(lldb) next
(lldb) finish

# View call stack
(lldb) bt

# Expression evaluation
(lldb) expr let result = someFunction()
```

## Popular Third-Party Libraries

### CocoaPods

```ruby
# Podfile
platform :ios, '14.0'
use_frameworks!

target 'MyApp' do
  pod 'Alamofire', '~> 5.6'
  pod 'Kingfisher', '~> 7.0'
  pod 'SnapKit', '~> 5.6.0'
  pod 'SwiftyJSON', '~> 5.0'
end
```

```bash
# Install dependencies
pod install

# Update dependencies
pod update
```

### Swift Package Manager

```swift
// Package.swift
dependencies: [
    .package(url: "https://github.com/Alamofire/Alamofire.git", from: "5.6.0"),
    .package(url: "https://github.com/onevcat/Kingfisher.git", from: "7.0.0")
]
```

### Common Libraries

```swift
// Alamofire - HTTP networking
import Alamofire

AF.request("https://api.example.com/users").responseDecodable(of: [User].self) { response in
    switch response.result {
    case .success(let users):
        print("Users: \(users)")
    case .failure(let error):
        print("Error: \(error)")
    }
}

// Kingfisher - Image loading and caching
import Kingfisher

imageView.kf.setImage(with: URL(string: imageURL))

// SnapKit - Auto Layout DSL
import SnapKit

view.snp.makeConstraints { make in
    make.top.equalTo(view.safeAreaLayoutGuide)
    make.leading.trailing.equalToSuperview().inset(16)
    make.height.equalTo(50)
}

// SwiftyJSON - JSON parsing
import SwiftyJSON

let json = JSON(data)
let name = json["user"]["name"].stringValue
```

## Official Resources

- [Apple Developer Documentation](https://developer.apple.com/documentation/)
- [iOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
- [Xcode Documentation](https://developer.apple.com/xcode/)
- [Swift Programming Language Guide](https://docs.swift.org/swift-book/)
- [App Store Review Guidelines](https://developer.apple.com/app-store/review/guidelines/)
- [WWDC Videos](https://developer.apple.com/videos/)
- [Apple Developer Forums](https://developer.apple.com/forums/)

## Community and Tools

- [Swift.org](https://swift.org/)
- [iOS Dev Directory](https://iosdevdirectory.com/)
- [Swift Package Index](https://swiftpackageindex.com/)
- [Awesome iOS](https://github.com/vsouza/awesome-ios)
- [RayWenderlich](https://www.raywenderlich.com/)
- [Hacking with Swift](https://www.hackingwithswift.com/)
- [SwiftUI Lab](https://swiftui-lab.com/)
- [iOS Dev Slack](https://ios-developers.io/)