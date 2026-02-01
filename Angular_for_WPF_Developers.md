# Angular for WPF Developers
## A Complete Transition Guide

---

## Introduction

This guide is designed for WPF (Windows Presentation Foundation) developers transitioning to Angular. Both frameworks share many architectural similarities, including MVVM-like patterns, two-way data binding, dependency injection, and component-based architecture. This guide highlights key differences, similarities, and best practices to help you leverage your WPF knowledge in the Angular world.

---

## Why Angular is Perfect for WPF Developers

Angular and WPF share remarkable similarities in their design philosophy:

- **Strong typing with TypeScript** (like C#)
- **Two-way data binding** (just like WPF)
- **Component-based architecture** (similar to UserControls)
- **Dependency Injection** (built-in, just like WPF)
- **Template syntax** (HTML templates like XAML)
- **Services and ViewModels** (similar patterns)
- **Observables** (like INotifyPropertyChanged)
- **CLI tooling** (like dotnet CLI)

---

## Key Similarities and Differences

| Aspect | WPF | Angular |
|--------|-----|---------|
| **Platform** | Desktop (Windows) | Web (Browser) |
| **Language** | C# + XAML | TypeScript + HTML |
| **Data Binding** | Two-way binding | Two-way binding (ngModel) |
| **Template Language** | XAML | HTML with Angular directives |
| **Dependency Injection** | Built-in (Constructor injection) | Built-in (Constructor injection) |
| **Component Model** | UserControls, Custom Controls | Components with decorators |
| **Styling** | Styles, Resources, XAML | CSS, SCSS, Angular Material |
| **Navigation** | Frames, Pages | Angular Router |
| **State Management** | INotifyPropertyChanged | RxJS Observables |
| **MVVM** | Native support | Similar pattern (Component as VM) |
| **CLI** | dotnet CLI | Angular CLI (ng) |

---

## Architecture Comparison

### WPF Architecture (MVVM)
```
View (XAML) ↔ ViewModel (C#) ↔ Model (C#)
     ↓              ↓
Data Binding   INotifyPropertyChanged
Commands       DependencyInjection
```

### Angular Architecture
```
Template (HTML) ↔ Component (TypeScript) ↔ Service (TypeScript)
     ↓                   ↓
Data Binding         Observables (RxJS)
Event Binding        DependencyInjection
```

---

## Hello World Comparison

### WPF (XAML + C#)

**MainWindow.xaml:**
```xml
<Window x:Class="WpfApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Hello WPF" Height="200" Width="400">
    <StackPanel VerticalAlignment="Center">
        <TextBlock Text="Hello, WPF!" 
                   FontSize="24" 
                   HorizontalAlignment="Center"/>
        <TextBox Text="{Binding Name, UpdateSourceTrigger=PropertyChanged}" 
                 Margin="10"/>
        <Button Content="Click Me" 
                Command="{Binding ClickCommand}" 
                Margin="10"/>
    </StackPanel>
</Window>
```

**MainWindow.xaml.cs:**
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        DataContext = new MainViewModel();
    }
}
```

**MainViewModel.cs:**
```csharp
public class MainViewModel : INotifyPropertyChanged
{
    private string _name;
    public string Name
    {
        get => _name;
        set
        {
            _name = value;
            OnPropertyChanged(nameof(Name));
        }
    }

    public ICommand ClickCommand { get; }

    public MainViewModel()
    {
        ClickCommand = new RelayCommand(() => 
            MessageBox.Show($"Hello, {Name}!"));
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Angular (TypeScript + HTML)

**app.component.html:**
```html
<div class="container">
  <h1 style="font-size: 24px; text-align: center">Hello, Angular!</h1>
  
  <input 
    [(ngModel)]="name" 
    placeholder="Enter your name"
    class="form-control">
  
  <button 
    (click)="onButtonClick()" 
    class="btn btn-primary">
    Click Me
  </button>
</div>
```

**app.component.ts:**
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  name: string = '';

  onButtonClick(): void {
    alert(`Hello, ${this.name}!`);
  }
}
```

**Key Similarities:**
- Both use two-way data binding (`Binding` in WPF, `[(ngModel)]` in Angular)
- Both separate template (XAML/HTML) from logic (C#/TypeScript)
- Both use event handlers (Commands/Click events)
- TypeScript syntax is very similar to C#

---

## Data Binding Comparison

### WPF: Two-Way Binding with INotifyPropertyChanged

**ViewModel:**
```csharp
public class UserViewModel : INotifyPropertyChanged
{
    private string _firstName;
    public string FirstName
    {
        get => _firstName;
        set
        {
            _firstName = value;
            OnPropertyChanged(nameof(FirstName));
            OnPropertyChanged(nameof(FullName));
        }
    }

    private string _lastName;
    public string LastName
    {
        get => _lastName;
        set
        {
            _lastName = value;
            OnPropertyChanged(nameof(LastName));
            OnPropertyChanged(nameof(FullName));
        }
    }

    public string FullName => $"{FirstName} {LastName}";

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**
```xml
<StackPanel>
    <TextBox Text="{Binding FirstName, UpdateSourceTrigger=PropertyChanged}"/>
    <TextBox Text="{Binding LastName, UpdateSourceTrigger=PropertyChanged}"/>
    <TextBlock Text="{Binding FullName}" FontWeight="Bold"/>
</StackPanel>
```

### Angular: Two-Way Binding with Property Binding

**Component:**
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
  styleUrls: ['./user.component.css']
})
export class UserComponent {
  firstName: string = '';
  lastName: string = '';

  // Computed property (like WPF)
  get fullName(): string {
    return `${this.firstName} ${this.lastName}`;
  }
}
```

**Template:**
```html
<div>
  <input [(ngModel)]="firstName" placeholder="First Name">
  <input [(ngModel)]="lastName" placeholder="Last Name">
  <p><strong>{{ fullName }}</strong></p>
</div>
```

**Key Differences:**
- Angular uses `[(ngModel)]` for two-way binding (banana in a box syntax)
- No need for INotifyPropertyChanged - Angular's change detection handles it
- Computed properties using getters (similar to WPF)
- Template interpolation with `{{ }}` instead of `{Binding}`

---

## Angular Binding Syntax (Critical!)

Angular has four types of data binding:

### 1. Interpolation (One-Way: Component → Template)
```html
<!-- Similar to WPF {Binding Property} -->
<h1>{{ title }}</h1>
<p>{{ user.name }}</p>
<span>{{ calculateTotal() }}</span>
```

### 2. Property Binding (One-Way: Component → Template)
```html
<!-- Similar to WPF Property="{Binding ...}" -->
<img [src]="imageUrl">
<button [disabled]="isLoading">Submit</button>
<div [class.active]="isActive">Content</div>
<div [style.color]="textColor">Colored text</div>
```

### 3. Event Binding (One-Way: Template → Component)
```html
<!-- Similar to WPF Click="MethodName" or Command="{Binding}" -->
<button (click)="onSave()">Save</button>
<input (input)="onSearch($event)">
<form (submit)="onSubmit($event)">
```

### 4. Two-Way Binding (Bidirectional)
```html
<!-- Similar to WPF Text="{Binding Name, Mode=TwoWay}" -->
<input [(ngModel)]="username">

<!-- Equivalent to: -->
<input [ngModel]="username" (ngModelChange)="username = $event">
```

---

## Component Creation

### WPF: UserControl

**UserInfoControl.xaml:**
```xml
<UserControl x:Class="WpfApp.UserInfoControl"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <StackPanel>
        <TextBlock Text="{Binding Name}" FontWeight="Bold"/>
        <TextBlock Text="{Binding Email}"/>
        <Button Content="Edit" Command="{Binding EditCommand}"/>
    </StackPanel>
</UserControl>
```

**UserInfoControl.xaml.cs:**
```csharp
public partial class UserInfoControl : UserControl
{
    public static readonly DependencyProperty NameProperty =
        DependencyProperty.Register("Name", typeof(string), 
            typeof(UserInfoControl));

    public string Name
    {
        get => (string)GetValue(NameProperty);
        set => SetValue(NameProperty, value);
    }

    // Similar for Email...

    public UserInfoControl()
    {
        InitializeComponent();
    }
}
```

### Angular: Component

**Create component using CLI:**
```bash
ng generate component user-info
# or shorthand:
ng g c user-info
```

**user-info.component.ts:**
```typescript
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-user-info',
  templateUrl: './user-info.component.html',
  styleUrls: ['./user-info.component.css']
})
export class UserInfoComponent {
  // Input properties (like DependencyProperty)
  @Input() name: string = '';
  @Input() email: string = '';

  // Output events (like RoutedEvent)
  @Output() edit = new EventEmitter<void>();

  onEdit(): void {
    this.edit.emit();
  }
}
```

**user-info.component.html:**
```html
<div class="user-info">
  <p><strong>{{ name }}</strong></p>
  <p>{{ email }}</p>
  <button (click)="onEdit()">Edit</button>
</div>
```

**user-info.component.css:**
```css
.user-info {
  border: 1px solid #ccc;
  padding: 10px;
  margin: 10px 0;
}

.user-info p {
  margin: 5px 0;
}
```

**Usage (Parent Component):**
```html
<!-- Similar to <local:UserInfoControl Name="John" Email="..." /> -->
<app-user-info 
  [name]="user.name" 
  [email]="user.email"
  (edit)="onEditUser()">
</app-user-info>
```

**Key Similarities:**
- `@Input()` decorator = DependencyProperty
- `@Output()` decorator = RoutedEvent
- Component selector = x:Class/x:Name
- Separate template and code files

---

## Lists and Data Display

### WPF: ItemsControl and ObservableCollection

**ViewModel:**
```csharp
public class MainViewModel : INotifyPropertyChanged
{
    public ObservableCollection<User> Users { get; set; }

    public ICommand DeleteUserCommand { get; }

    public MainViewModel()
    {
        Users = new ObservableCollection<User>
        {
            new User { Id = 1, Name = "John", Email = "john@example.com" },
            new User { Id = 2, Name = "Jane", Email = "jane@example.com" }
        };

        DeleteUserCommand = new RelayCommand<User>(user => Users.Remove(user));
    }
}
```

**XAML:**
```xml
<ListView ItemsSource="{Binding Users}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="{Binding Name}" Width="150"/>
                <TextBlock Text="{Binding Email}" Width="200"/>
                <Button Content="Delete" 
                        Command="{Binding DataContext.DeleteUserCommand, 
                                RelativeSource={RelativeSource AncestorType=ListView}}"
                        CommandParameter="{Binding}"/>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

### Angular: *ngFor Directive

**Component:**
```typescript
import { Component } from '@angular/core';

interface User {
  id: number;
  name: string;
  email: string;
}

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.css']
})
export class UserListComponent {
  users: User[] = [
    { id: 1, name: 'John', email: 'john@example.com' },
    { id: 2, name: 'Jane', email: 'jane@example.com' }
  ];

  deleteUser(user: User): void {
    this.users = this.users.filter(u => u.id !== user.id);
  }

  // Tracking function for better performance (like x:Key in WPF)
  trackByUserId(index: number, user: User): number {
    return user.id;
  }
}
```

**Template:**
```html
<div class="user-list">
  <div 
    *ngFor="let user of users; trackBy: trackByUserId" 
    class="user-item">
    <span class="user-name">{{ user.name }}</span>
    <span class="user-email">{{ user.email }}</span>
    <button (click)="deleteUser(user)" class="btn-delete">Delete</button>
  </div>

  <!-- Empty state (like DataTrigger in WPF) -->
  <div *ngIf="users.length === 0" class="empty-state">
    No users found
  </div>
</div>
```

**Key Similarities:**
- `*ngFor` = ItemsSource binding
- `trackBy` = x:Key (improves performance)
- Array methods instead of ObservableCollection (but works similarly)

---

## Dependency Injection

This is where Angular and WPF are almost identical!

### WPF: Constructor Injection

**Service:**
```csharp
public interface IUserService
{
    Task<List<User>> GetUsersAsync();
}

public class UserService : IUserService
{
    private readonly HttpClient _httpClient;

    public UserService(HttpClient httpClient)
    {
        _httpClient = httpClient;
    }

    public async Task<List<User>> GetUsersAsync()
    {
        var response = await _httpClient.GetStringAsync("api/users");
        return JsonSerializer.Deserialize<List<User>>(response);
    }
}
```

**Registration (App.xaml.cs or Startup):**
```csharp
services.AddSingleton<IUserService, UserService>();
```

**Usage in ViewModel:**
```csharp
public class MainViewModel : INotifyPropertyChanged
{
    private readonly IUserService _userService;

    public MainViewModel(IUserService userService)
    {
        _userService = userService;
    }

    public async Task LoadUsersAsync()
    {
        var users = await _userService.GetUsersAsync();
        // Update UI
    }
}
```

### Angular: Constructor Injection (Nearly Identical!)

**Service:**
```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

export interface User {
  id: number;
  name: string;
  email: string;
}

@Injectable({
  providedIn: 'root' // Singleton, like services.AddSingleton
})
export class UserService {
  constructor(private http: HttpClient) { }

  getUsers(): Observable<User[]> {
    return this.http.get<User[]>('api/users');
  }
}
```

**Usage in Component:**
```typescript
import { Component, OnInit } from '@angular/core';
import { UserService, User } from './user.service';

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html'
})
export class UserListComponent implements OnInit {
  users: User[] = [];
  isLoading = false;

  // Constructor injection - exactly like WPF!
  constructor(private userService: UserService) { }

  ngOnInit(): void {
    this.loadUsers();
  }

  loadUsers(): void {
    this.isLoading = true;
    this.userService.getUsers().subscribe({
      next: (users) => {
        this.users = users;
        this.isLoading = false;
      },
      error: (error) => {
        console.error('Error loading users:', error);
        this.isLoading = false;
      }
    });
  }
}
```

**Key Similarities:**
- Constructor injection works the same way
- `@Injectable()` decorator = service registration
- `providedIn: 'root'` = Singleton lifetime
- TypeScript interfaces work like C# interfaces

---

## Observables vs INotifyPropertyChanged

### WPF: INotifyPropertyChanged

```csharp
public class UserViewModel : INotifyPropertyChanged
{
    private readonly IUserService _userService;
    private ObservableCollection<User> _users;

    public ObservableCollection<User> Users
    {
        get => _users;
        set
        {
            _users = value;
            OnPropertyChanged(nameof(Users));
        }
    }

    public async Task LoadUsersAsync()
    {
        var users = await _userService.GetUsersAsync();
        Users = new ObservableCollection<User>(users);
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Angular: RxJS Observables

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { Observable, Subject } from 'rxjs';
import { takeUntil } from 'rxjs/operators';
import { UserService, User } from './user.service';

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html'
})
export class UserListComponent implements OnInit, OnDestroy {
  // Observable pattern (reactive)
  users$: Observable<User[]>;
  
  // Or traditional approach
  users: User[] = [];

  private destroy$ = new Subject<void>();

  constructor(private userService: UserService) {
    // Direct observable assignment
    this.users$ = this.userService.getUsers();
  }

  ngOnInit(): void {
    // Subscribe to observable
    this.userService.getUsers()
      .pipe(takeUntil(this.destroy$)) // Automatic unsubscribe
      .subscribe(users => {
        this.users = users;
      });
  }

  ngOnDestroy(): void {
    // Cleanup (like IDisposable in C#)
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

**Template with Async Pipe (Recommended):**
```html
<!-- Async pipe handles subscription/unsubscription automatically -->
<div *ngFor="let user of users$ | async">
  {{ user.name }}
</div>

<!-- Loading state -->
<div *ngIf="!(users$ | async)">Loading...</div>
```

**Key Differences:**
- Observables are push-based (vs pull-based properties)
- `async` pipe handles subscriptions automatically
- RxJS operators provide powerful data transformation

---

## Commands vs Methods

### WPF: ICommand Pattern

**ViewModel:**
```csharp
public class MainViewModel : INotifyPropertyChanged
{
    private string _searchText;
    public string SearchText
    {
        get => _searchText;
        set
        {
            _searchText = value;
            OnPropertyChanged(nameof(SearchText));
            SearchCommand.RaiseCanExecuteChanged();
        }
    }

    public RelayCommand SearchCommand { get; }
    public RelayCommand ClearCommand { get; }

    public MainViewModel()
    {
        SearchCommand = new RelayCommand(
            execute: () => PerformSearch(),
            canExecute: () => !string.IsNullOrEmpty(SearchText)
        );

        ClearCommand = new RelayCommand(() => SearchText = string.Empty);
    }

    private void PerformSearch()
    {
        // Search logic
    }
}
```

**XAML:**
```xml
<StackPanel>
    <TextBox Text="{Binding SearchText, UpdateSourceTrigger=PropertyChanged}"/>
    <Button Content="Search" Command="{Binding SearchCommand}"/>
    <Button Content="Clear" Command="{Binding ClearCommand}"/>
</StackPanel>
```

### Angular: Direct Method Binding

**Component:**
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-search',
  templateUrl: './search.component.html'
})
export class SearchComponent {
  searchText: string = '';

  performSearch(): void {
    if (this.canSearch()) {
      // Search logic
      console.log('Searching for:', this.searchText);
    }
  }

  canSearch(): boolean {
    return this.searchText?.trim().length > 0;
  }

  clear(): void {
    this.searchText = '';
  }
}
```

**Template:**
```html
<div>
  <input [(ngModel)]="searchText" placeholder="Search...">
  <button 
    (click)="performSearch()" 
    [disabled]="!canSearch()">
    Search
  </button>
  <button (click)="clear()">Clear</button>
</div>
```

**Key Differences:**
- Angular uses direct method calls instead of ICommand
- `[disabled]` binding handles CanExecute logic
- Simpler syntax, less boilerplate

---

## Routing and Navigation

### WPF: Frame Navigation

**MainWindow.xaml:**
```xml
<Window>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <Menu Grid.Row="0">
            <MenuItem Header="Home" Click="NavigateHome"/>
            <MenuItem Header="Users" Click="NavigateUsers"/>
            <MenuItem Header="Settings" Click="NavigateSettings"/>
        </Menu>

        <Frame Grid.Row="1" x:Name="MainFrame" NavigationUIVisibility="Hidden"/>
    </Grid>
</Window>
```

**Code-behind:**
```csharp
private void NavigateHome(object sender, RoutedEventArgs e)
{
    MainFrame.Navigate(new HomePage());
}

private void NavigateUsers(object sender, RoutedEventArgs e)
{
    MainFrame.Navigate(new UsersPage());
}
```

### Angular: Router Module

**app-routing.module.ts:**
```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { UsersComponent } from './users/users.component';
import { UserDetailComponent } from './user-detail/user-detail.component';
import { SettingsComponent } from './settings/settings.component';

const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'users', component: UsersComponent },
  { path: 'users/:id', component: UserDetailComponent }, // Route parameters
  { path: 'settings', component: SettingsComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

**app.component.html:**
```html
<nav>
  <a routerLink="/home" routerLinkActive="active">Home</a>
  <a routerLink="/users" routerLinkActive="active">Users</a>
  <a routerLink="/settings" routerLinkActive="active">Settings</a>
</nav>

<!-- Router outlet (like Frame in WPF) -->
<router-outlet></router-outlet>
```

**Programmatic Navigation:**
```typescript
import { Component } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-users',
  templateUrl: './users.component.html'
})
export class UsersComponent {
  constructor(private router: Router) { }

  navigateToUserDetail(userId: number): void {
    // Similar to NavigationService.Navigate
    this.router.navigate(['/users', userId]);
  }

  goBack(): void {
    // Similar to NavigationService.GoBack
    window.history.back();
  }
}
```

**Accessing Route Parameters:**
```typescript
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-user-detail',
  templateUrl: './user-detail.component.html'
})
export class UserDetailComponent implements OnInit {
  userId: number;

  constructor(private route: ActivatedRoute) { }

  ngOnInit(): void {
    // Get route parameter (like query string in WPF)
    this.route.params.subscribe(params => {
      this.userId = +params['id']; // + converts string to number
    });

    // Or snapshot for one-time access
    this.userId = +this.route.snapshot.paramMap.get('id');
  }
}
```

---

## Directives (Like Attached Properties)

### WPF: Attached Properties

```xml
<Grid>
    <TextBlock Text="Header" Grid.Row="0" Grid.Column="0"/>
    <TextBlock Text="Content" Grid.Row="1" Grid.Column="1"/>
</Grid>
```

### Angular: Built-in Directives

**Structural Directives (change DOM structure):**
```html
<!-- *ngIf (like Visibility in WPF) -->
<div *ngIf="isLoggedIn">Welcome back!</div>
<div *ngIf="isLoggedIn; else loginBlock">Welcome!</div>
<ng-template #loginBlock>
  <div>Please log in</div>
</ng-template>

<!-- *ngFor (like ItemsControl) -->
<div *ngFor="let item of items; let i = index">
  {{ i }}: {{ item.name }}
</div>

<!-- *ngSwitch (like DataTrigger) -->
<div [ngSwitch]="userRole">
  <div *ngSwitchCase="'admin'">Admin Panel</div>
  <div *ngSwitchCase="'user'">User Dashboard</div>
  <div *ngSwitchDefault>Guest View</div>
</div>
```

**Attribute Directives (change appearance/behavior):**
```html
<!-- ngClass (dynamic class binding) -->
<div [ngClass]="{'active': isActive, 'disabled': isDisabled}">Content</div>

<!-- ngStyle (dynamic style binding) -->
<div [ngStyle]="{'color': textColor, 'font-size.px': fontSize}">Styled text</div>

<!-- Class binding -->
<div [class.active]="isActive">Content</div>

<!-- Style binding -->
<div [style.color]="isError ? 'red' : 'black'">Message</div>
```

**Custom Directive (like attached behavior in WPF):**
```typescript
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input() appHighlight: string = 'yellow';

  constructor(private el: ElementRef) { }

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.appHighlight);
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight('');
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

**Usage:**
```html
<p appHighlight="lightblue">Hover over me!</p>
```

---

## Pipes (Like Value Converters)

### WPF: IValueConverter

```csharp
public class DateFormatConverter : IValueConverter
{
    public object Convert(object value, Type targetType, 
        object parameter, CultureInfo culture)
    {
        if (value is DateTime date)
        {
            return date.ToString("MM/dd/yyyy");
        }
        return value;
    }

    public object ConvertBack(object value, Type targetType, 
        object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

**XAML:**
```xml
<Window.Resources>
    <local:DateFormatConverter x:Key="DateFormatter"/>
</Window.Resources>

<TextBlock Text="{Binding BirthDate, Converter={StaticResource DateFormatter}}"/>
```

### Angular: Pipes

**Built-in Pipes:**
```html
<!-- Date pipe -->
<p>{{ birthDate | date:'MM/dd/yyyy' }}</p>
<p>{{ birthDate | date:'fullDate' }}</p>

<!-- Currency pipe -->
<p>{{ price | currency:'USD':'symbol':'1.2-2' }}</p>

<!-- Uppercase/Lowercase -->
<p>{{ name | uppercase }}</p>
<p>{{ name | lowercase }}</p>

<!-- Decimal/Percent -->
<p>{{ value | number:'1.2-2' }}</p>
<p>{{ ratio | percent }}</p>

<!-- JSON (for debugging) -->
<pre>{{ user | json }}</pre>

<!-- Async pipe (unwraps observables) -->
<div *ngFor="let user of users$ | async">
  {{ user.name }}
</div>
```

**Custom Pipe:**
```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'truncate'
})
export class TruncatePipe implements PipeTransform {
  transform(value: string, limit: number = 50, trail: string = '...'): string {
    if (!value) return '';
    return value.length > limit ? value.substring(0, limit) + trail : value;
  }
}
```

**Usage:**
```html
<p>{{ longText | truncate:100:'...' }}</p>
```

**Chaining Pipes:**
```html
<p>{{ birthDate | date:'fullDate' | uppercase }}</p>
<p>{{ price | currency:'USD' | lowercase }}</p>
```

---

## Forms and Validation

### WPF: IDataErrorInfo

**ViewModel:**
```csharp
public class UserViewModel : INotifyPropertyChanged, IDataErrorInfo
{
    private string _email;
    public string Email
    {
        get => _email;
        set
        {
            _email = value;
            OnPropertyChanged(nameof(Email));
        }
    }

    private int _age;
    public int Age
    {
        get => _age;
        set
        {
            _age = value;
            OnPropertyChanged(nameof(Age));
        }
    }

    public string Error => null;

    public string this[string columnName]
    {
        get
        {
            switch (columnName)
            {
                case nameof(Email):
                    if (string.IsNullOrEmpty(Email))
                        return "Email is required";
                    if (!Email.Contains("@"))
                        return "Invalid email format";
                    break;
                case nameof(Age):
                    if (Age < 18)
                        return "Must be 18 or older";
                    break;
            }
            return null;
        }
    }
}
```

### Angular: Template-Driven Forms

**Component:**
```typescript
import { Component } from '@angular/core';

interface UserForm {
  email: string;
  age: number;
}

@Component({
  selector: 'app-user-form',
  templateUrl: './user-form.component.html'
})
export class UserFormComponent {
  user: UserForm = {
    email: '',
    age: 0
  };

  onSubmit(): void {
    console.log('Form submitted:', this.user);
  }
}
```

**Template:**
```html
<form #userForm="ngForm" (ngSubmit)="onSubmit()">
  <div>
    <label>Email:</label>
    <input 
      type="email"
      name="email"
      [(ngModel)]="user.email"
      #email="ngModel"
      required
      email
      [class.error]="email.invalid && email.touched">
    
    <!-- Validation messages -->
    <div *ngIf="email.invalid && email.touched" class="error-message">
      <div *ngIf="email.errors?.['required']">Email is required</div>
      <div *ngIf="email.errors?.['email']">Invalid email format</div>
    </div>
  </div>

  <div>
    <label>Age:</label>
    <input 
      type="number"
      name="age"
      [(ngModel)]="user.age"
      #age="ngModel"
      required
      min="18"
      [class.error]="age.invalid && age.touched">
    
    <div *ngIf="age.invalid && age.touched" class="error-message">
      <div *ngIf="age.errors?.['required']">Age is required</div>
      <div *ngIf="age.errors?.['min']">Must be 18 or older</div>
    </div>
  </div>

  <button type="submit" [disabled]="userForm.invalid">Submit</button>
</form>
```

### Angular: Reactive Forms (More like WPF)

**Component:**
```typescript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-user-form',
  templateUrl: './user-form.component.html'
})
export class UserFormComponent implements OnInit {
  userForm: FormGroup;

  constructor(private fb: FormBuilder) { }

  ngOnInit(): void {
    this.userForm = this.fb.group({
      email: ['', [Validators.required, Validators.email]],
      age: ['', [Validators.required, Validators.min(18)]],
      password: ['', [Validators.required, Validators.minLength(6)]]
    });

    // Subscribe to value changes (like PropertyChanged)
    this.userForm.valueChanges.subscribe(values => {
      console.log('Form changed:', values);
    });
  }

  get email() { return this.userForm.get('email'); }
  get age() { return this.userForm.get('age'); }
  get password() { return this.userForm.get('password'); }

  onSubmit(): void {
    if (this.userForm.valid) {
      console.log('Form submitted:', this.userForm.value);
    }
  }
}
```

**Template:**
```html
<form [formGroup]="userForm" (ngSubmit)="onSubmit()">
  <div>
    <label>Email:</label>
    <input 
      type="email"
      formControlName="email"
      [class.error]="email.invalid && email.touched">
    
    <div *ngIf="email.invalid && email.touched" class="error-message">
      <div *ngIf="email.errors?.['required']">Email is required</div>
      <div *ngIf="email.errors?.['email']">Invalid email format</div>
    </div>
  </div>

  <div>
    <label>Age:</label>
    <input 
      type="number"
      formControlName="age"
      [class.error]="age.invalid && age.touched">
    
    <div *ngIf="age.invalid && age.touched" class="error-message">
      <div *ngIf="age.errors?.['required']">Age is required</div>
      <div *ngIf="age.errors?.['min']">Must be 18 or older</div>
    </div>
  </div>

  <button type="submit" [disabled]="userForm.invalid">Submit</button>
</form>
```

**Custom Validators:**
```typescript
import { AbstractControl, ValidationErrors, ValidatorFn } from '@angular/forms';

export function passwordMatchValidator(): ValidatorFn {
  return (control: AbstractControl): ValidationErrors | null => {
    const password = control.get('password');
    const confirmPassword = control.get('confirmPassword');

    if (!password || !confirmPassword) {
      return null;
    }

    return password.value === confirmPassword.value ? null : { passwordMismatch: true };
  };
}

// Usage
this.userForm = this.fb.group({
  password: ['', [Validators.required, Validators.minLength(6)]],
  confirmPassword: ['', Validators.required]
}, { validators: passwordMatchValidator() });
```

---

## Lifecycle Hooks (Like WPF Events)

### WPF: Control Lifecycle

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        Loaded += MainWindow_Loaded;
        Unloaded += MainWindow_Unloaded;
    }

    private void MainWindow_Loaded(object sender, RoutedEventArgs e)
    {
        // Initialize data
    }

    private void MainWindow_Unloaded(object sender, RoutedEventArgs e)
    {
        // Cleanup
    }
}
```

### Angular: Lifecycle Hooks

```typescript
import { Component, OnInit, OnDestroy, OnChanges, SimpleChanges, 
         AfterViewInit, Input } from '@angular/core';
import { Subscription } from 'rxjs';

@Component({
  selector: 'app-user-detail',
  templateUrl: './user-detail.component.html'
})
export class UserDetailComponent implements OnInit, OnDestroy, OnChanges, AfterViewInit {
  @Input() userId: number;
  
  private subscription: Subscription;

  constructor() {
    // Constructor - like C# constructor
    console.log('Constructor called');
  }

  ngOnChanges(changes: SimpleChanges): void {
    // Called when input properties change
    console.log('Input changed:', changes);
    if (changes['userId']) {
      this.loadUser(this.userId);
    }
  }

  ngOnInit(): void {
    // Called once after component initialization (like Loaded event)
    console.log('Component initialized');
    this.loadData();
  }

  ngAfterViewInit(): void {
    // Called after view is fully initialized
    console.log('View initialized');
  }

  ngOnDestroy(): void {
    // Called before component is destroyed (like Unloaded event)
    console.log('Component destroyed');
    // Cleanup subscriptions
    if (this.subscription) {
      this.subscription.unsubscribe();
    }
  }

  private loadData(): void {
    // Load initial data
  }

  private loadUser(id: number): void {
    // Load user by ID
  }
}
```

**Common Lifecycle Hooks:**

| Hook | WPF Equivalent | Purpose |
|------|----------------|---------|
| `ngOnChanges` | PropertyChanged event | Input properties changed |
| `ngOnInit` | Loaded event | Component initialized |
| `ngAfterViewInit` | Loaded (after render) | View fully rendered |
| `ngOnDestroy` | Unloaded event | Cleanup before destruction |

---

## Styling and Themes

### WPF: Styles and Resources

**App.xaml:**
```xml
<Application.Resources>
    <Style x:Key="PrimaryButton" TargetType="Button">
        <Setter Property="Background" Value="#007ACC"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Padding" Value="10,5"/>
        <Style.Triggers>
            <Trigger Property="IsMouseOver" Value="True">
                <Setter Property="Background" Value="#005A9E"/>
            </Trigger>
        </Style.Triggers>
    </Style>
</Application.Resources>
```

**Usage:**
```xml
<Button Content="Click Me" Style="{StaticResource PrimaryButton}"/>
```

### Angular: Component Styles

**Global Styles (styles.css):**
```css
/* Similar to App.xaml resources */
:root {
  --primary-color: #007ACC;
  --primary-hover: #005A9E;
}

.btn-primary {
  background-color: var(--primary-color);
  color: white;
  font-size: 14px;
  padding: 10px 5px;
  border: none;
  cursor: pointer;
}

.btn-primary:hover {
  background-color: var(--primary-hover);
}
```

**Component-Scoped Styles:**
```typescript
@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
  styleUrls: ['./user.component.css'],
  // Or inline styles
  styles: [`
    .user-card {
      border: 1px solid #ccc;
      padding: 10px;
    }
  `]
})
export class UserComponent { }
```

**user.component.css:**
```css
/* These styles are scoped to this component only */
.user-card {
  border: 1px solid #ccc;
  padding: 15px;
  margin: 10px 0;
  border-radius: 4px;
}

.user-card:hover {
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.user-name {
  font-weight: bold;
  font-size: 18px;
}
```

**Using SCSS (like CSS preprocessor):**
```scss
// user.component.scss
$primary-color: #007ACC;
$spacing: 10px;

.user-card {
  border: 1px solid #ccc;
  padding: $spacing + 5px;
  
  &:hover {
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  }
  
  .user-name {
    font-weight: bold;
    color: $primary-color;
  }
}
```

---

## Angular Material (Like MahApps or MaterialDesign)

**Installation:**
```bash
ng add @angular/material
```

**Usage:**
```typescript
import { MatButtonModule } from '@angular/material/button';
import { MatInputModule } from '@angular/material/input';
import { MatCardModule } from '@angular/material/card';

@NgModule({
  imports: [
    MatButtonModule,
    MatInputModule,
    MatCardModule
  ]
})
export class AppModule { }
```

**Template:**
```html
<mat-card>
  <mat-card-header>
    <mat-card-title>User Information</mat-card-title>
  </mat-card-header>
  
  <mat-card-content>
    <mat-form-field>
      <mat-label>Email</mat-label>
      <input matInput [(ngModel)]="email" type="email">
    </mat-form-field>
    
    <mat-form-field>
      <mat-label>Password</mat-label>
      <input matInput [(ngModel)]="password" type="password">
    </mat-form-field>
  </mat-card-content>
  
  <mat-card-actions>
    <button mat-raised-button color="primary" (click)="onSubmit()">
      Submit
    </button>
    <button mat-button (click)="onCancel()">Cancel</button>
  </mat-card-actions>
</mat-card>
```

---

## HTTP Communication

### WPF: HttpClient

```csharp
public class UserService
{
    private readonly HttpClient _httpClient;

    public UserService(HttpClient httpClient)
    {
        _httpClient = httpClient;
    }

    public async Task<List<User>> GetUsersAsync()
    {
        var response = await _httpClient.GetAsync("https://api.example.com/users");
        response.EnsureSuccessStatusCode();
        
        var json = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<List<User>>(json);
    }

    public async Task<User> CreateUserAsync(User user)
    {
        var json = JsonSerializer.Serialize(user);
        var content = new StringContent(json, Encoding.UTF8, "application/json");
        
        var response = await _httpClient.PostAsync("https://api.example.com/users", content);
        response.EnsureSuccessStatusCode();
        
        var responseJson = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<User>(responseJson);
    }
}
```

### Angular: HttpClient

**Service:**
```typescript
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders, HttpParams } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError, retry } from 'rxjs/operators';

export interface User {
  id: number;
  name: string;
  email: string;
}

@Injectable({
  providedIn: 'root'
})
export class UserService {
  private apiUrl = 'https://api.example.com/users';

  constructor(private http: HttpClient) { }

  // GET request
  getUsers(): Observable<User[]> {
    return this.http.get<User[]>(this.apiUrl)
      .pipe(
        retry(3), // Retry failed requests
        catchError(this.handleError)
      );
  }

  // GET with parameters
  getUserById(id: number): Observable<User> {
    return this.http.get<User>(`${this.apiUrl}/${id}`)
      .pipe(catchError(this.handleError));
  }

  // GET with query parameters
  searchUsers(query: string): Observable<User[]> {
    const params = new HttpParams().set('q', query);
    return this.http.get<User[]>(this.apiUrl, { params })
      .pipe(catchError(this.handleError));
  }

  // POST request
  createUser(user: User): Observable<User> {
    const headers = new HttpHeaders({ 'Content-Type': 'application/json' });
    return this.http.post<User>(this.apiUrl, user, { headers })
      .pipe(catchError(this.handleError));
  }

  // PUT request
  updateUser(user: User): Observable<User> {
    return this.http.put<User>(`${this.apiUrl}/${user.id}`, user)
      .pipe(catchError(this.handleError));
  }

  // DELETE request
  deleteUser(id: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`)
      .pipe(catchError(this.handleError));
  }

  private handleError(error: any): Observable<never> {
    console.error('An error occurred:', error);
    return throwError(() => new Error('Something went wrong'));
  }
}
```

**Component Usage:**
```typescript
import { Component, OnInit } from '@angular/core';
import { UserService, User } from './user.service';

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html'
})
export class UserListComponent implements OnInit {
  users: User[] = [];
  isLoading = false;
  errorMessage = '';

  constructor(private userService: UserService) { }

  ngOnInit(): void {
    this.loadUsers();
  }

  loadUsers(): void {
    this.isLoading = true;
    this.userService.getUsers().subscribe({
      next: (users) => {
        this.users = users;
        this.isLoading = false;
      },
      error: (error) => {
        this.errorMessage = error.message;
        this.isLoading = false;
      }
    });
  }

  createUser(user: User): void {
    this.userService.createUser(user).subscribe({
      next: (newUser) => {
        this.users.push(newUser);
      },
      error: (error) => {
        console.error('Error creating user:', error);
      }
    });
  }

  deleteUser(id: number): void {
    this.userService.deleteUser(id).subscribe({
      next: () => {
        this.users = this.users.filter(u => u.id !== id);
      },
      error: (error) => {
        console.error('Error deleting user:', error);
      }
    });
  }
}
```

---

## State Management

### WPF: Shared ViewModels

```csharp
// Singleton service for shared state
public class AppState : INotifyPropertyChanged
{
    private static AppState _instance;
    public static AppState Instance => _instance ??= new AppState();

    private User _currentUser;
    public User CurrentUser
    {
        get => _currentUser;
        set
        {
            _currentUser = value;
            OnPropertyChanged(nameof(CurrentUser));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Angular: Service-Based State

**state.service.ts:**
```typescript
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable } from 'rxjs';

export interface User {
  id: number;
  name: string;
  email: string;
}

@Injectable({
  providedIn: 'root'
})
export class StateService {
  // BehaviorSubject holds current value and emits to new subscribers
  private currentUserSubject = new BehaviorSubject<User | null>(null);
  
  // Expose as observable (read-only)
  currentUser$: Observable<User | null> = this.currentUserSubject.asObservable();

  // Getter for current value
  get currentUser(): User | null {
    return this.currentUserSubject.value;
  }

  // Setter
  setCurrentUser(user: User | null): void {
    this.currentUserSubject.next(user);
  }

  // Clear state
  clearCurrentUser(): void {
    this.currentUserSubject.next(null);
  }
}
```

**Component Usage:**
```typescript
import { Component, OnInit } from '@angular/core';
import { StateService, User } from './state.service';

@Component({
  selector: 'app-header',
  template: `
    <div *ngIf="currentUser$ | async as user">
      Welcome, {{ user.name }}!
    </div>
  `
})
export class HeaderComponent implements OnInit {
  currentUser$ = this.stateService.currentUser$;

  constructor(private stateService: StateService) { }

  ngOnInit(): void {
    // Subscribe to changes
    this.currentUser$.subscribe(user => {
      console.log('Current user changed:', user);
    });
  }
}

// Another component
@Component({
  selector: 'app-login',
  templateUrl: './login.component.html'
})
export class LoginComponent {
  constructor(private stateService: StateService) { }

  login(email: string, password: string): void {
    // After successful login
    const user: User = { id: 1, name: 'John Doe', email };
    this.stateService.setCurrentUser(user);
  }

  logout(): void {
    this.stateService.clearCurrentUser();
  }
}
```

### NgRx (Redux for Angular - Advanced)

**Installation:**
```bash
ng add @ngrx/store
```

**Similar to WPF MVVM Light Messenger pattern but more structured**

---

## Project Structure Comparison

### WPF Project Structure:
```
WpfApp/
├── WpfApp.sln
├── WpfApp/
│   ├── WpfApp.csproj
│   ├── App.xaml
│   ├── App.xaml.cs
│   ├── Views/
│   │   ├── MainWindow.xaml
│   │   ├── MainWindow.xaml.cs
│   │   └── UserControls/
│   ├── ViewModels/
│   │   └── MainViewModel.cs
│   ├── Models/
│   │   └── User.cs
│   ├── Services/
│   │   └── UserService.cs
│   └── Converters/
│       └── BoolToVisibilityConverter.cs
```

### Angular Project Structure:
```
angular-app/
├── package.json
├── angular.json
├── tsconfig.json
├── src/
│   ├── main.ts
│   ├── index.html
│   ├── styles.css
│   ├── app/
│   │   ├── app.module.ts
│   │   ├── app-routing.module.ts
│   │   ├── app.component.ts
│   │   ├── app.component.html
│   │   ├── app.component.css
│   │   ├── components/
│   │   │   ├── header/
│   │   │   │   ├── header.component.ts
│   │   │   │   ├── header.component.html
│   │   │   │   └── header.component.css
│   │   │   └── user-card/
│   │   ├── pages/
│   │   │   ├── home/
│   │   │   └── users/
│   │   ├── services/
│   │   │   └── user.service.ts
│   │   ├── models/
│   │   │   └── user.model.ts
│   │   ├── pipes/
│   │   │   └── truncate.pipe.ts
│   │   └── directives/
│   │       └── highlight.directive.ts
│   └── environments/
│       ├── environment.ts
│       └── environment.prod.ts
```

---

## Angular CLI (Like dotnet CLI)

### Common Commands:

```bash
# Create new Angular application
ng new my-app

# Start development server (like dotnet run)
ng serve
ng serve --open  # Opens browser automatically

# Generate components
ng generate component user-list
ng g c user-list  # Shorthand

# Generate service
ng generate service user
ng g s user

# Generate module
ng generate module users --routing
ng g m users --routing

# Generate pipe
ng generate pipe truncate
ng g p truncate

# Generate directive
ng generate directive highlight
ng g d highlight

# Build for production (like dotnet publish)
ng build --prod
ng build --configuration production

# Run tests (like dotnet test)
ng test

# Run linter
ng lint
```

---

## Testing

### WPF Testing (MSTest)

```csharp
[TestClass]
public class UserViewModelTests
{
    [TestMethod]
    public void LoadUsers_ShouldPopulateUsersList()
    {
        // Arrange
        var mockService = new Mock<IUserService>();
        mockService.Setup(s => s.GetUsersAsync())
            .ReturnsAsync(new List<User> { new User { Name = "John" } });
        
        var viewModel = new UserViewModel(mockService.Object);

        // Act
        await viewModel.LoadUsersAsync();

        // Assert
        Assert.AreEqual(1, viewModel.Users.Count);
        Assert.AreEqual("John", viewModel.Users[0].Name);
    }
}
```

### Angular Testing (Jasmine + Karma)

**Component Test:**
```typescript
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { UserListComponent } from './user-list.component';
import { UserService } from './user.service';
import { of } from 'rxjs';

describe('UserListComponent', () => {
  let component: UserListComponent;
  let fixture: ComponentFixture<UserListComponent>;
  let mockUserService: jasmine.SpyObj<UserService>;

  beforeEach(() => {
    // Create mock service
    mockUserService = jasmine.createSpyObj('UserService', ['getUsers']);

    TestBed.configureTestingModule({
      declarations: [ UserListComponent ],
      providers: [
        { provide: UserService, useValue: mockUserService }
      ]
    });

    fixture = TestBed.createComponent(UserListComponent);
    component = fixture.componentInstance;
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should load users on init', () => {
    // Arrange
    const mockUsers = [
      { id: 1, name: 'John', email: 'john@example.com' }
    ];
    mockUserService.getUsers.and.returnValue(of(mockUsers));

    // Act
    component.ngOnInit();

    // Assert
    expect(component.users.length).toBe(1);
    expect(component.users[0].name).toBe('John');
  });
});
```

**Service Test:**
```typescript
import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { UserService, User } from './user.service';

describe('UserService', () => {
  let service: UserService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [UserService]
    });

    service = TestBed.inject(UserService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    httpMock.verify();
  });

  it('should fetch users', () => {
    const mockUsers: User[] = [
      { id: 1, name: 'John', email: 'john@example.com' }
    ];

    service.getUsers().subscribe(users => {
      expect(users.length).toBe(1);
      expect(users[0].name).toBe('John');
    });

    const req = httpMock.expectOne('https://api.example.com/users');
    expect(req.request.method).toBe('GET');
    req.flush(mockUsers);
  });
});
```

---

## Key Concepts Translation

| WPF Concept | Angular Equivalent |
|-------------|-------------------|
| **UserControl** | Component (`@Component`) |
| **DependencyProperty** | `@Input()` property |
| **RoutedEvent** | `@Output()` EventEmitter |
| **DataContext** | Component class properties |
| **Binding** | `[(ngModel)]`, `[property]`, `{{}}` |
| **INotifyPropertyChanged** | Automatic change detection |
| **ICommand** | Method with `(click)="method()"` |
| **IValueConverter** | Pipe (`@Pipe`) |
| **DataTemplate** | Structural directives (`*ngFor`, `*ngIf`) |
| **Style/Resource** | CSS classes, component styles |
| **Trigger** | `[class]`, `[ngClass]`, `[ngStyle]` |
| **ObservableCollection** | Array with change detection |
| **IDisposable** | `ngOnDestroy()` lifecycle hook |
| **Frame/NavigationService** | Router (`RouterModule`) |
| **ViewModel** | Component class |
| **Dependency Injection** | Constructor injection (same!) |

---

## Best Practices

### 1. Use TypeScript Strictly

```typescript
// Enable strict mode in tsconfig.json
{
  "compilerOptions": {
    "strict": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true
  }
}

// Define interfaces for all data models
interface User {
  id: number;
  name: string;
  email: string;
  createdAt?: Date; // Optional property
}

// Use type annotations
function processUser(user: User): string {
  return user.name.toUpperCase();
}
```

### 2. Follow Angular Style Guide

```typescript
// Use OnPush change detection for performance
@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserListComponent { }

// Use trackBy with *ngFor
<div *ngFor="let user of users; trackBy: trackByUserId">
  {{ user.name }}
</div>

trackByUserId(index: number, user: User): number {
  return user.id;
}

// Unsubscribe from observables
ngOnDestroy(): void {
  this.subscription.unsubscribe();
  // Or use takeUntil pattern
  this.destroy$.next();
  this.destroy$.complete();
}
```

### 3. Smart vs Presentational Components

```typescript
// Smart Component (Container) - has logic and services
@Component({
  selector: 'app-user-list-container',
  template: `
    <app-user-list-presentation
      [users]="users"
      [isLoading]="isLoading"
      (delete)="onDeleteUser($event)">
    </app-user-list-presentation>
  `
})
export class UserListContainerComponent {
  users: User[] = [];
  isLoading = false;

  constructor(private userService: UserService) { }

  ngOnInit(): void {
    this.loadUsers();
  }

  loadUsers(): void {
    this.isLoading = true;
    this.userService.getUsers().subscribe(users => {
      this.users = users;
      this.isLoading = false;
    });
  }

  onDeleteUser(user: User): void {
    this.userService.deleteUser(user.id).subscribe();
  }
}

// Presentational Component - only displays data
@Component({
  selector: 'app-user-list-presentation',
  templateUrl: './user-list-presentation.component.html'
})
export class UserListPresentationComponent {
  @Input() users: User[] = [];
  @Input() isLoading = false;
  @Output() delete = new EventEmitter<User>();

  onDelete(user: User): void {
    this.delete.emit(user);
  }
}
```

---

## Learning Path

### Week 1-2: Fundamentals
- ✅ Learn TypeScript basics (very similar to C#)
- ✅ Understand Angular CLI
- ✅ Master component creation
- ✅ Practice data binding (two-way, one-way, event)
- ✅ Learn template syntax

### Week 3-4: Core Concepts
- ✅ Master directives (*ngIf, *ngFor, *ngSwitch)
- ✅ Understand pipes (value converters)
- ✅ Learn dependency injection
- ✅ Practice with services
- ✅ Explore component communication (@Input, @Output)

### Week 5-6: Intermediate
- ✅ Implement routing and navigation
- ✅ Learn forms (template-driven and reactive)
- ✅ Understand RxJS and Observables
- ✅ Practice HTTP communication
- ✅ Explore lifecycle hooks

### Week 7-8: Advanced
- ✅ State management (services or NgRx)
- ✅ Custom directives and pipes
- ✅ Performance optimization
- ✅ Testing with Jasmine/Karma
- ✅ Build and deploy application

---

## Essential Libraries

| Category | Library | Purpose | WPF Equivalent |
|----------|---------|---------|----------------|
| **UI Components** | Angular Material | Material Design components | MahApps, MaterialDesignInXaml |
| **Forms** | Reactive Forms | Form handling and validation | Data binding, IDataErrorInfo |
| **HTTP** | HttpClient | API communication | HttpClient |
| **Routing** | Router | Navigation | Frame, NavigationService |
| **State Management** | NgRx | Redux pattern for Angular | MVVM Light Messenger |
| **Date/Time** | date-fns, Moment.js | Date manipulation | DateTime |
| **Charts** | ng2-charts, ngx-charts | Data visualization | LiveCharts, OxyPlot |
| **Icons** | Font Awesome, Material Icons | Icon libraries | Built-in icons |
| **Animations** | Angular Animations | Animations | Storyboards |

### Installation Examples:

```bash
# Angular Material
ng add @angular/material

# NgRx (state management)
ng add @ngrx/store

# Charts
npm install ng2-charts chart.js

# Date utilities
npm install date-fns
```

---

## Resources

### Official Documentation:
- **Angular Docs**: https://angular.io/docs
- **Angular CLI**: https://angular.io/cli
- **RxJS**: https://rxjs.dev/
- **Angular Material**: https://material.angular.io/

### Learning Platforms:
- **Angular University**: https://angular-university.io/
- **Tour of Heroes Tutorial**: https://angular.io/tutorial
- **Pluralsight Angular Path**: https://www.pluralsight.com/paths/angular

### Tools:
- **VS Code**: https://code.visualstudio.com/
- **Angular Language Service**: VS Code extension
- **Augury**: Chrome DevTools extension for Angular

---

## Conclusion

As a WPF developer, you'll find Angular remarkably familiar. Both frameworks share:

- **Strong typing** (C# vs TypeScript)
- **Two-way data binding**
- **Component-based architecture**
- **Dependency injection**
- **Similar project structure and patterns**
- **Template-based UI (XAML vs HTML)**

The main differences are:
- Web platform vs Desktop
- HTML/CSS instead of XAML
- RxJS Observables instead of INotifyPropertyChanged
- Different syntax but same concepts

### Key Takeaways:

✅ **TypeScript is very similar to C#** - You'll feel right at home  
✅ **Two-way binding works the same** - `[(ngModel)]` is your friend  
✅ **Dependency Injection is identical** - Constructor injection patterns  
✅ **Components = UserControls** - Same component model  
✅ **@Input/@Output = DependencyProperty/RoutedEvent** - Property communication  
✅ **Pipes = Value Converters** - Data transformation  
✅ **Services = ViewModels/Services** - Business logic layer  
✅ **Angular CLI = dotnet CLI** - Similar tooling experience  

Your WPF experience gives you a massive advantage in learning Angular. The architectural patterns, MVVM principles, and development practices translate directly. Focus on learning the syntax differences, and you'll be productive very quickly!

---

**Happy coding with Angular! 🅰️**
