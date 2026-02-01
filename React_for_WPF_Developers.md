# React for WPF Developers
## A Complete Transition Guide

---

## Introduction

This guide is designed for WPF (Windows Presentation Foundation) developers transitioning to React. Both frameworks are used for building user interfaces, but they approach the problem differently. This guide highlights key differences, similarities, and best practices to help you become productive with React quickly.

---

## Key Differences Overview

| Aspect | WPF | React |
|--------|-----|-------|
| **Platform** | Desktop (Windows) | Web (Browser) |
| **Language** | C# + XAML | JavaScript/TypeScript + JSX |
| **Rendering** | Native Windows controls | Virtual DOM → Browser DOM |
| **Data Binding** | Two-way binding (default) | One-way data flow (unidirectional) |
| **Styling** | Styles, Templates, Resources | CSS, CSS-in-JS, Styled Components |
| **State Management** | INotifyPropertyChanged, DependencyProperty | useState, useReducer, Context, Redux |
| **Component Model** | UserControls, Custom Controls | Functional Components, Class Components |
| **Layout** | Grid, StackPanel, DockPanel, Canvas | Flexbox, CSS Grid, div elements |
| **Navigation** | Frames, Pages, Windows | React Router |
| **MVVM** | Built-in support | Requires libraries/patterns |

---

## Fundamental Concepts Comparison

### WPF Architecture
```
View (XAML) ←→ ViewModel (C#) ←→ Model (C#)
     ↓
Data Binding
INotifyPropertyChanged
Commands
```

### React Architecture
```
Component (JSX + JS/TS) → Props → Child Component
     ↓
State (useState/useReducer)
     ↓
Re-render on state/props change
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
        <Button Content="Click Me" 
                Click="Button_Click" 
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
    }

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        MessageBox.Show("Button clicked!");
    }
}
```

### React (JSX + JavaScript/TypeScript)

**App.jsx:**
```jsx
import React, { useState } from 'react';
import './App.css';

function App() {
  const [message, setMessage] = useState('');

  const handleClick = () => {
    setMessage('Button clicked!');
    alert('Button clicked!');
  };

  return (
    <div className="app-container">
      <h1 style={{ fontSize: '24px', textAlign: 'center' }}>
        Hello, React!
      </h1>
      <button onClick={handleClick} style={{ margin: '10px' }}>
        Click Me
      </button>
      {message && <p>{message}</p>}
    </div>
  );
}

export default App;
```

**Key Differences:**
- React uses JSX (JavaScript XML) instead of XAML
- Event handlers are functions, not methods with sender/EventArgs
- No code-behind files - everything in one component
- Inline styles use JavaScript objects, not strings

---

## Data Binding Comparison

### WPF: Two-Way Binding

**ViewModel:**
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

    private string _greeting;
    public string Greeting
    {
        get => _greeting;
        set
        {
            _greeting = value;
            OnPropertyChanged(nameof(Greeting));
        }
    }

    public ICommand UpdateGreetingCommand { get; }

    public MainViewModel()
    {
        UpdateGreetingCommand = new RelayCommand(UpdateGreeting);
    }

    private void UpdateGreeting()
    {
        Greeting = $"Hello, {Name}!";
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**
```xml
<Window.DataContext>
    <local:MainViewModel/>
</Window.DataContext>

<StackPanel>
    <TextBox Text="{Binding Name, UpdateSourceTrigger=PropertyChanged}" />
    <Button Content="Update Greeting" Command="{Binding UpdateGreetingCommand}" />
    <TextBlock Text="{Binding Greeting}" FontSize="20" />
</StackPanel>
```

### React: One-Way Data Flow with State

```jsx
import React, { useState } from 'react';

function GreetingComponent() {
  const [name, setName] = useState('');
  const [greeting, setGreeting] = useState('');

  const updateGreeting = () => {
    setGreeting(`Hello, ${name}!`);
  };

  return (
    <div>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Enter your name"
      />
      <button onClick={updateGreeting}>
        Update Greeting
      </button>
      <h2 style={{ fontSize: '20px' }}>{greeting}</h2>
    </div>
  );
}

export default GreetingComponent;
```

**Key Differences:**
- React uses `useState` hook instead of INotifyPropertyChanged
- Manual event handlers (`onChange`) instead of automatic two-way binding
- State updates trigger re-renders automatically
- No need for PropertyChanged events

---

## Component Composition

### WPF: UserControls

**UserInfoControl.xaml:**
```xml
<UserControl x:Class="WpfApp.UserInfoControl"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <StackPanel>
        <TextBlock Text="{Binding Name}" FontWeight="Bold"/>
        <TextBlock Text="{Binding Email}" />
        <TextBlock Text="{Binding Age}" />
    </StackPanel>
</UserControl>
```

**UserInfoControl.xaml.cs:**
```csharp
public partial class UserInfoControl : UserControl
{
    public static readonly DependencyProperty NameProperty =
        DependencyProperty.Register("Name", typeof(string), 
            typeof(UserInfoControl), new PropertyMetadata(string.Empty));

    public string Name
    {
        get => (string)GetValue(NameProperty);
        set => SetValue(NameProperty, value);
    }

    // Similar for Email, Age...
    
    public UserInfoControl()
    {
        InitializeComponent();
    }
}
```

**Usage:**
```xml
<local:UserInfoControl Name="John Doe" Email="john@example.com" Age="30" />
```

### React: Functional Components with Props

**UserInfoComponent.jsx:**
```jsx
import React from 'react';
import PropTypes from 'prop-types';

function UserInfo({ name, email, age }) {
  return (
    <div className="user-info">
      <p style={{ fontWeight: 'bold' }}>{name}</p>
      <p>{email}</p>
      <p>{age}</p>
    </div>
  );
}

// PropTypes for type checking (like DependencyProperty validation)
UserInfo.propTypes = {
  name: PropTypes.string.isRequired,
  email: PropTypes.string.isRequired,
  age: PropTypes.number.isRequired
};

// Default props (like default values in DependencyProperty)
UserInfo.defaultProps = {
  name: '',
  email: '',
  age: 0
};

export default UserInfo;
```

**Usage:**
```jsx
function App() {
  return (
    <div>
      <UserInfo name="John Doe" email="john@example.com" age={30} />
      <UserInfo name="Jane Smith" email="jane@example.com" age={25} />
    </div>
  );
}
```

**With TypeScript (recommended):**
```tsx
interface UserInfoProps {
  name: string;
  email: string;
  age: number;
}

const UserInfo: React.FC<UserInfoProps> = ({ name, email, age }) => {
  return (
    <div className="user-info">
      <p style={{ fontWeight: 'bold' }}>{name}</p>
      <p>{email}</p>
      <p>{age}</p>
    </div>
  );
};

export default UserInfo;
```

---

## Lists and Data Display

### WPF: ItemsControl and ListView

**XAML:**
```xml
<ListView ItemsSource="{Binding Users}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal" Margin="5">
                <TextBlock Text="{Binding Name}" FontWeight="Bold" Width="150"/>
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
            new User { Name = "John", Email = "john@example.com" },
            new User { Name = "Jane", Email = "jane@example.com" }
        };
        
        DeleteUserCommand = new RelayCommand<User>(DeleteUser);
    }

    private void DeleteUser(User user)
    {
        Users.Remove(user);
    }
}
```

### React: map() Function

```jsx
import React, { useState } from 'react';

function UserList() {
  const [users, setUsers] = useState([
    { id: 1, name: 'John', email: 'john@example.com' },
    { id: 2, name: 'Jane', email: 'jane@example.com' }
  ]);

  const deleteUser = (userId) => {
    setUsers(users.filter(user => user.id !== userId));
  };

  return (
    <div className="user-list">
      {users.map(user => (
        <div key={user.id} style={{ 
          display: 'flex', 
          alignItems: 'center', 
          margin: '5px' 
        }}>
          <span style={{ fontWeight: 'bold', width: '150px' }}>
            {user.name}
          </span>
          <span style={{ width: '200px' }}>
            {user.email}
          </span>
          <button onClick={() => deleteUser(user.id)}>
            Delete
          </button>
        </div>
      ))}
    </div>
  );
}

export default UserList;
```

**Key Differences:**
- Use `map()` instead of ItemsControl/ListView
- `key` prop is required for list items (like `x:Key` in WPF)
- Array methods (`filter`, `map`, `reduce`) instead of ObservableCollection
- State updates create new arrays (immutability)

---

## Styling Comparison

### WPF: Styles and Resources

**App.xaml:**
```xml
<Application.Resources>
    <Style x:Key="PrimaryButton" TargetType="Button">
        <Setter Property="Background" Value="#007ACC"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="Padding" Value="10,5"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Cursor" Value="Hand"/>
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
<Button Content="Click Me" Style="{StaticResource PrimaryButton}" />
```

### React: Multiple Styling Approaches

#### 1. CSS Files (Most Common)

**Button.css:**
```css
.primary-button {
  background-color: #007ACC;
  color: white;
  padding: 10px 5px;
  font-size: 14px;
  cursor: pointer;
  border: none;
  border-radius: 4px;
}

.primary-button:hover {
  background-color: #005A9E;
}

.primary-button:active {
  transform: scale(0.98);
}
```

**Button.jsx:**
```jsx
import React from 'react';
import './Button.css';

function PrimaryButton({ children, onClick }) {
  return (
    <button className="primary-button" onClick={onClick}>
      {children}
    </button>
  );
}

export default PrimaryButton;
```

#### 2. Inline Styles (JavaScript Objects)

```jsx
function PrimaryButton({ children, onClick }) {
  const buttonStyle = {
    backgroundColor: '#007ACC',
    color: 'white',
    padding: '10px 5px',
    fontSize: '14px',
    cursor: 'pointer',
    border: 'none',
    borderRadius: '4px'
  };

  return (
    <button style={buttonStyle} onClick={onClick}>
      {children}
    </button>
  );
}
```

#### 3. CSS Modules (Scoped Styling)

**Button.module.css:**
```css
.primaryButton {
  background-color: #007ACC;
  color: white;
  padding: 10px 5px;
  font-size: 14px;
  cursor: pointer;
  border: none;
  border-radius: 4px;
}

.primaryButton:hover {
  background-color: #005A9E;
}
```

**Button.jsx:**
```jsx
import React from 'react';
import styles from './Button.module.css';

function PrimaryButton({ children, onClick }) {
  return (
    <button className={styles.primaryButton} onClick={onClick}>
      {children}
    </button>
  );
}

export default PrimaryButton;
```

#### 4. Styled Components (CSS-in-JS)

```jsx
import styled from 'styled-components';

const PrimaryButton = styled.button`
  background-color: #007ACC;
  color: white;
  padding: 10px 5px;
  font-size: 14px;
  cursor: pointer;
  border: none;
  border-radius: 4px;

  &:hover {
    background-color: #005A9E;
  }

  &:active {
    transform: scale(0.98);
  }
`;

// Usage
<PrimaryButton onClick={handleClick}>Click Me</PrimaryButton>
```

---

## Layout Comparison

### WPF Layouts

**Grid:**
```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="200"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>

    <TextBlock Grid.Row="0" Grid.Column="0" Text="Header"/>
    <TextBlock Grid.Row="1" Grid.Column="1" Text="Content"/>
</Grid>
```

**StackPanel:**
```xml
<StackPanel Orientation="Vertical" Spacing="10">
    <TextBlock Text="Item 1"/>
    <TextBlock Text="Item 2"/>
    <TextBlock Text="Item 3"/>
</StackPanel>
```

### React Layouts (CSS Flexbox & Grid)

**CSS Grid:**
```css
.grid-container {
  display: grid;
  grid-template-rows: auto 1fr auto;
  grid-template-columns: 200px 1fr;
  height: 100vh;
  gap: 10px;
}

.header {
  grid-row: 1;
  grid-column: 1;
}

.content {
  grid-row: 2;
  grid-column: 2;
}
```

```jsx
function GridLayout() {
  return (
    <div className="grid-container">
      <div className="header">Header</div>
      <div className="content">Content</div>
    </div>
  );
}
```

**Flexbox (Similar to StackPanel):**
```css
.stack-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.horizontal-stack {
  display: flex;
  flex-direction: row;
  gap: 10px;
}
```

```jsx
function StackLayout() {
  return (
    <div className="stack-container">
      <div>Item 1</div>
      <div>Item 2</div>
      <div>Item 3</div>
    </div>
  );
}
```

---

## Commands vs Event Handlers

### WPF: ICommand Pattern

**ViewModel:**
```csharp
public class MainViewModel : INotifyPropertyChanged
{
    public ICommand SaveCommand { get; }
    public ICommand DeleteCommand { get; }

    public MainViewModel()
    {
        SaveCommand = new RelayCommand(Save, CanSave);
        DeleteCommand = new RelayCommand(Delete);
    }

    private void Save()
    {
        // Save logic
    }

    private bool CanSave()
    {
        return !string.IsNullOrEmpty(Name);
    }

    private void Delete()
    {
        // Delete logic
    }
}
```

**XAML:**
```xml
<Button Content="Save" Command="{Binding SaveCommand}"/>
<Button Content="Delete" Command="{Binding DeleteCommand}"/>
```

### React: Event Handlers

```jsx
import React, { useState } from 'react';

function FormComponent() {
  const [name, setName] = useState('');

  const handleSave = () => {
    // Save logic
    console.log('Saving:', name);
  };

  const handleDelete = () => {
    // Delete logic
    console.log('Deleting');
  };

  const canSave = () => {
    return name.trim() !== '';
  };

  return (
    <div>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <button onClick={handleSave} disabled={!canSave()}>
        Save
      </button>
      <button onClick={handleDelete}>
        Delete
      </button>
    </div>
  );
}

export default FormComponent;
```

---

## State Management

### WPF: ViewModels and Data Context

**Simple ViewModel:**
```csharp
public class CounterViewModel : INotifyPropertyChanged
{
    private int _count = 0;
    
    public int Count
    {
        get => _count;
        set
        {
            _count = value;
            OnPropertyChanged(nameof(Count));
        }
    }

    public ICommand IncrementCommand { get; }

    public CounterViewModel()
    {
        IncrementCommand = new RelayCommand(() => Count++);
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### React: useState Hook

**Simple State:**
```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

export default Counter;
```

**Complex State with useReducer (Similar to Redux):**
```jsx
import React, { useReducer } from 'react';

// Similar to Command pattern in WPF
const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}

export default Counter;
```

---

## Dependency Properties vs Props

### WPF: DependencyProperty

```csharp
public class CustomControl : Control
{
    public static readonly DependencyProperty TitleProperty =
        DependencyProperty.Register(
            "Title",
            typeof(string),
            typeof(CustomControl),
            new PropertyMetadata("Default Title", OnTitleChanged));

    public string Title
    {
        get => (string)GetValue(TitleProperty);
        set => SetValue(TitleProperty, value);
    }

    private static void OnTitleChanged(DependencyObject d, 
        DependencyPropertyChangedEventArgs e)
    {
        var control = (CustomControl)d;
        // React to property change
    }
}
```

**Usage:**
```xml
<local:CustomControl Title="My Title" />
```

### React: Props and useEffect

```jsx
import React, { useEffect } from 'react';
import PropTypes from 'prop-types';

function CustomComponent({ title }) {
  // Similar to OnTitleChanged
  useEffect(() => {
    console.log('Title changed:', title);
    // React to prop change
  }, [title]); // Dependency array - re-run when title changes

  return (
    <div>
      <h1>{title}</h1>
    </div>
  );
}

CustomComponent.propTypes = {
  title: PropTypes.string
};

CustomComponent.defaultProps = {
  title: 'Default Title'
};

export default CustomComponent;
```

**With TypeScript:**
```tsx
interface CustomComponentProps {
  title?: string;
}

const CustomComponent: React.FC<CustomComponentProps> = ({ 
  title = 'Default Title' 
}) => {
  useEffect(() => {
    console.log('Title changed:', title);
  }, [title]);

  return (
    <div>
      <h1>{title}</h1>
    </div>
  );
};

export default CustomComponent;
```

---

## Navigation

### WPF: Frames and Pages

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
            <MenuItem Header="About" Click="NavigateAbout"/>
        </Menu>

        <Frame Grid.Row="1" x:Name="MainFrame" NavigationUIVisibility="Hidden"/>
    </Grid>
</Window>
```

**MainWindow.xaml.cs:**
```csharp
private void NavigateHome(object sender, RoutedEventArgs e)
{
    MainFrame.Navigate(new HomePage());
}

private void NavigateAbout(object sender, RoutedEventArgs e)
{
    MainFrame.Navigate(new AboutPage());
}
```

### React: React Router

**Installation:**
```bash
npm install react-router-dom
```

**App.jsx:**
```jsx
import React from 'react';
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom';
import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';
import UserPage from './pages/UserPage';

function App() {
  return (
    <Router>
      <nav>
        <ul>
          <li><Link to="/">Home</Link></li>
          <li><Link to="/about">About</Link></li>
          <li><Link to="/user/123">User Profile</Link></li>
        </ul>
      </nav>

      <Routes>
        <Route path="/" element={<HomePage />} />
        <Route path="/about" element={<AboutPage />} />
        <Route path="/user/:id" element={<UserPage />} />
      </Routes>
    </Router>
  );
}

export default App;
```

**UserPage.jsx (Using route parameters):**
```jsx
import React from 'react';
import { useParams, useNavigate } from 'react-router-dom';

function UserPage() {
  const { id } = useParams(); // Similar to query parameters in WPF
  const navigate = useNavigate();

  const goBack = () => {
    navigate(-1); // Like NavigationService.GoBack()
  };

  const goToHome = () => {
    navigate('/'); // Programmatic navigation
  };

  return (
    <div>
      <h1>User Profile: {id}</h1>
      <button onClick={goBack}>Go Back</button>
      <button onClick={goToHome}>Go Home</button>
    </div>
  );
}

export default UserPage;
```

---

## Data Templates vs Conditional Rendering

### WPF: DataTemplates

```xml
<ListView ItemsSource="{Binding Items}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <StackPanel>
                <TextBlock Text="{Binding Title}" FontWeight="Bold"/>
                <ContentPresenter>
                    <ContentPresenter.ContentTemplate>
                        <DataTemplate>
                            <Border Background="LightBlue" 
                                    Visibility="{Binding IsActive, Converter={StaticResource BoolToVisibility}}">
                                <TextBlock Text="Active Item"/>
                            </Border>
                        </DataTemplate>
                    </ContentPresenter.ContentTemplate>
                </ContentPresenter>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

### React: Conditional Rendering

```jsx
function ItemList({ items }) {
  return (
    <div className="item-list">
      {items.map(item => (
        <div key={item.id}>
          <h3 style={{ fontWeight: 'bold' }}>{item.title}</h3>
          
          {/* Conditional rendering - multiple approaches */}
          
          {/* 1. && operator (short-circuit) */}
          {item.isActive && (
            <div style={{ backgroundColor: 'lightblue', padding: '10px' }}>
              Active Item
            </div>
          )}

          {/* 2. Ternary operator */}
          {item.isActive ? (
            <div style={{ backgroundColor: 'lightblue' }}>Active</div>
          ) : (
            <div style={{ backgroundColor: 'lightgray' }}>Inactive</div>
          )}

          {/* 3. Function-based rendering */}
          {renderStatus(item)}
        </div>
      ))}
    </div>
  );
}

function renderStatus(item) {
  if (item.isActive && item.isPremium) {
    return <div>Premium Active User</div>;
  } else if (item.isActive) {
    return <div>Active User</div>;
  } else {
    return <div>Inactive User</div>;
  }
}
```

---

## Value Converters vs Functions

### WPF: IValueConverter

```csharp
public class BoolToVisibilityConverter : IValueConverter
{
    public object Convert(object value, Type targetType, 
        object parameter, CultureInfo culture)
    {
        if (value is bool boolValue)
        {
            return boolValue ? Visibility.Visible : Visibility.Collapsed;
        }
        return Visibility.Collapsed;
    }

    public object ConvertBack(object value, Type targetType, 
        object parameter, CultureInfo culture)
    {
        if (value is Visibility visibility)
        {
            return visibility == Visibility.Visible;
        }
        return false;
    }
}
```

**XAML:**
```xml
<Window.Resources>
    <local:BoolToVisibilityConverter x:Key="BoolToVisibility"/>
</Window.Resources>

<TextBlock Text="Visible" 
           Visibility="{Binding IsActive, Converter={StaticResource BoolToVisibility}}"/>
```

### React: Helper Functions

```jsx
function MyComponent({ isActive, price, date }) {
  // Simple conversion functions
  const getVisibility = (isVisible) => {
    return isVisible ? 'block' : 'none';
  };

  const formatCurrency = (amount) => {
    return new Intl.NumberFormat('en-US', {
      style: 'currency',
      currency: 'USD'
    }).format(amount);
  };

  const formatDate = (dateString) => {
    return new Date(dateString).toLocaleDateString();
  };

  return (
    <div>
      <p style={{ display: getVisibility(isActive) }}>
        I'm visible when active
      </p>
      <p>Price: {formatCurrency(price)}</p>
      <p>Date: {formatDate(date)}</p>
    </div>
  );
}

// Or create reusable utility file
// utils/formatters.js
export const formatCurrency = (amount) => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'
  }).format(amount);
};

export const formatDate = (dateString) => {
  return new Date(dateString).toLocaleDateString();
};

// Usage
import { formatCurrency, formatDate } from './utils/formatters';
```

---

## Forms and Validation

### WPF: IDataErrorInfo

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

    public string Error => null;

    public string this[string columnName]
    {
        get
        {
            if (columnName == nameof(Email))
            {
                if (string.IsNullOrEmpty(Email))
                    return "Email is required";
                if (!Email.Contains("@"))
                    return "Invalid email format";
            }
            return null;
        }
    }
}
```

**XAML:**
```xml
<TextBox Text="{Binding Email, ValidatesOnDataErrors=True, UpdateSourceTrigger=PropertyChanged}">
    <TextBox.Style>
        <Style TargetType="TextBox">
            <Style.Triggers>
                <Trigger Property="Validation.HasError" Value="True">
                    <Setter Property="BorderBrush" Value="Red"/>
                </Trigger>
            </Style.Triggers>
        </Style>
    </TextBox.Style>
</TextBox>
```

### React: Form Validation

**Basic validation:**
```jsx
import React, { useState } from 'react';

function RegistrationForm() {
  const [formData, setFormData] = useState({
    email: '',
    password: ''
  });
  const [errors, setErrors] = useState({});

  const validateEmail = (email) => {
    if (!email) return 'Email is required';
    if (!email.includes('@')) return 'Invalid email format';
    return '';
  };

  const validatePassword = (password) => {
    if (!password) return 'Password is required';
    if (password.length < 6) return 'Password must be at least 6 characters';
    return '';
  };

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));

    // Validate on change
    let error = '';
    if (name === 'email') error = validateEmail(value);
    if (name === 'password') error = validatePassword(value);

    setErrors(prev => ({
      ...prev,
      [name]: error
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();

    const emailError = validateEmail(formData.email);
    const passwordError = validatePassword(formData.password);

    if (emailError || passwordError) {
      setErrors({
        email: emailError,
        password: passwordError
      });
      return;
    }

    // Submit form
    console.log('Form submitted:', formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          placeholder="Email"
          style={{ borderColor: errors.email ? 'red' : '#ccc' }}
        />
        {errors.email && (
          <span style={{ color: 'red', fontSize: '12px' }}>
            {errors.email}
          </span>
        )}
      </div>

      <div>
        <input
          type="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
          placeholder="Password"
          style={{ borderColor: errors.password ? 'red' : '#ccc' }}
        />
        {errors.password && (
          <span style={{ color: 'red', fontSize: '12px' }}>
            {errors.password}
          </span>
        )}
      </div>

      <button type="submit">Register</button>
    </form>
  );
}

export default RegistrationForm;
```

**Using Formik library (recommended for complex forms):**
```jsx
import React from 'react';
import { Formik, Form, Field, ErrorMessage } from 'formik';
import * as Yup from 'yup';

const validationSchema = Yup.object({
  email: Yup.string()
    .email('Invalid email format')
    .required('Email is required'),
  password: Yup.string()
    .min(6, 'Password must be at least 6 characters')
    .required('Password is required')
});

function RegistrationForm() {
  return (
    <Formik
      initialValues={{ email: '', password: '' }}
      validationSchema={validationSchema}
      onSubmit={(values) => {
        console.log('Form submitted:', values);
      }}
    >
      {({ errors, touched }) => (
        <Form>
          <div>
            <Field
              type="email"
              name="email"
              placeholder="Email"
              style={{ 
                borderColor: errors.email && touched.email ? 'red' : '#ccc' 
              }}
            />
            <ErrorMessage 
              name="email" 
              component="span" 
              style={{ color: 'red', fontSize: '12px' }}
            />
          </div>

          <div>
            <Field
              type="password"
              name="password"
              placeholder="Password"
              style={{ 
                borderColor: errors.password && touched.password ? 'red' : '#ccc' 
              }}
            />
            <ErrorMessage 
              name="password" 
              component="span" 
              style={{ color: 'red', fontSize: '12px' }}
            />
          </div>

          <button type="submit">Register</button>
        </Form>
      )}
    </Formik>
  );
}

export default RegistrationForm;
```

---

## Lifecycle and Effects

### WPF: Events and Overrides

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        Loaded += MainWindow_Loaded;
        Closing += MainWindow_Closing;
    }

    private void MainWindow_Loaded(object sender, RoutedEventArgs e)
    {
        // Similar to componentDidMount in React
        LoadData();
    }

    private void MainWindow_Closing(object sender, CancelEventArgs e)
    {
        // Similar to componentWillUnmount in React
        CleanupResources();
    }

    private async void LoadData()
    {
        var data = await FetchDataAsync();
        DataContext = new MainViewModel(data);
    }
}
```

### React: useEffect Hook

```jsx
import React, { useState, useEffect } from 'react';

function MyComponent() {
  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(true);

  // Runs on mount (similar to Loaded event)
  useEffect(() => {
    console.log('Component mounted');
    loadData();

    // Cleanup function (similar to Closing event)
    return () => {
      console.log('Component will unmount');
      cleanupResources();
    };
  }, []); // Empty dependency array = run once on mount

  // Runs when specific value changes
  useEffect(() => {
    console.log('Data changed:', data);
  }, [data]); // Re-run when data changes

  const loadData = async () => {
    setIsLoading(true);
    try {
      const response = await fetch('https://api.example.com/data');
      const result = await response.json();
      setData(result);
    } catch (error) {
      console.error('Error loading data:', error);
    } finally {
      setIsLoading(false);
    }
  };

  const cleanupResources = () => {
    // Cleanup code (cancel subscriptions, timers, etc.)
  };

  if (isLoading) {
    return <div>Loading...</div>;
  }

  return (
    <div>
      <h1>Data loaded!</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}

export default MyComponent;
```

**Common useEffect patterns:**
```jsx
// Run once on mount
useEffect(() => {
  // Setup
  return () => {
    // Cleanup
  };
}, []);

// Run when dependency changes
useEffect(() => {
  // Do something when userId changes
}, [userId]);

// Timer example (similar to DispatcherTimer in WPF)
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Tick');
  }, 1000);

  return () => clearInterval(timer); // Cleanup
}, []);

// Event listener example
useEffect(() => {
  const handleResize = () => {
    console.log('Window resized');
  };

  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);
```

---

## Async Operations

### WPF: async/await with UI Thread

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    private bool _isLoading;
    public bool IsLoading
    {
        get => _isLoading;
        set
        {
            _isLoading = value;
            OnPropertyChanged(nameof(IsLoading));
        }
    }

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

    public ICommand LoadUsersCommand { get; }

    public MainViewModel()
    {
        LoadUsersCommand = new RelayCommand(async () => await LoadUsersAsync());
    }

    private async Task LoadUsersAsync()
    {
        IsLoading = true;
        try
        {
            using var client = new HttpClient();
            var json = await client.GetStringAsync("https://api.example.com/users");
            var users = JsonSerializer.Deserialize<List<User>>(json);
            
            // UI thread update is automatic
            Users = new ObservableCollection<User>(users);
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error: {ex.Message}");
        }
        finally
        {
            IsLoading = false;
        }
    }
}
```

### React: async/await with useEffect

```jsx
import React, { useState, useEffect } from 'react';

function UserList() {
  const [users, setUsers] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const loadUsers = async () => {
    setIsLoading(true);
    setError(null);
    
    try {
      const response = await fetch('https://api.example.com/users');
      
      if (!response.ok) {
        throw new Error('Failed to fetch users');
      }
      
      const data = await response.json();
      setUsers(data);
    } catch (err) {
      setError(err.message);
      console.error('Error loading users:', err);
    } finally {
      setIsLoading(false);
    }
  };

  // Load on component mount
  useEffect(() => {
    loadUsers();
  }, []);

  if (isLoading) {
    return <div className="loading">Loading...</div>;
  }

  if (error) {
    return (
      <div className="error">
        <p>Error: {error}</p>
        <button onClick={loadUsers}>Retry</button>
      </div>
    );
  }

  return (
    <div className="user-list">
      <button onClick={loadUsers}>Refresh</button>
      {users.map(user => (
        <div key={user.id} className="user-card">
          <h3>{user.name}</h3>
          <p>{user.email}</p>
        </div>
      ))}
    </div>
  );
}

export default UserList;
```

**Using custom hooks for reusability:**
```jsx
// hooks/useFetch.js
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      setIsLoading(true);
      setError(null);

      try {
        const response = await fetch(url);
        if (!response.ok) throw new Error('Failed to fetch');
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false);
      }
    };

    if (url) {
      fetchData();
    }
  }, [url]);

  return { data, isLoading, error };
}

export default useFetch;

// Usage
function UserList() {
  const { data: users, isLoading, error } = useFetch('https://api.example.com/users');

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      {users?.map(user => (
        <div key={user.id}>{user.name}</div>
      ))}
    </div>
  );
}
```

---

## Context and Global State

### WPF: Shared ViewModels / Services

```csharp
// Singleton service
public class UserService
{
    private static UserService _instance;
    public static UserService Instance => _instance ??= new UserService();

    public User CurrentUser { get; set; }

    public event EventHandler<User> UserChanged;

    public void SetCurrentUser(User user)
    {
        CurrentUser = user;
        UserChanged?.Invoke(this, user);
    }
}

// Usage in ViewModel
public class MainViewModel : INotifyPropertyChanged
{
    public MainViewModel()
    {
        UserService.Instance.UserChanged += OnUserChanged;
    }

    private void OnUserChanged(object sender, User user)
    {
        // Update UI
        OnPropertyChanged(nameof(CurrentUser));
    }

    public User CurrentUser => UserService.Instance.CurrentUser;
}
```

### React: Context API

**UserContext.js:**
```jsx
import React, { createContext, useState, useContext } from 'react';

// Create context
const UserContext = createContext();

// Provider component
export function UserProvider({ children }) {
  const [currentUser, setCurrentUser] = useState(null);
  const [isAuthenticated, setIsAuthenticated] = useState(false);

  const login = (user) => {
    setCurrentUser(user);
    setIsAuthenticated(true);
  };

  const logout = () => {
    setCurrentUser(null);
    setIsAuthenticated(false);
  };

  const value = {
    currentUser,
    isAuthenticated,
    login,
    logout
  };

  return (
    <UserContext.Provider value={value}>
      {children}
    </UserContext.Provider>
  );
}

// Custom hook to use the context
export function useUser() {
  const context = useContext(UserContext);
  if (!context) {
    throw new Error('useUser must be used within UserProvider');
  }
  return context;
}
```

**App.js (Setup):**
```jsx
import React from 'react';
import { UserProvider } from './contexts/UserContext';
import Header from './components/Header';
import MainContent from './components/MainContent';

function App() {
  return (
    <UserProvider>
      <div className="app">
        <Header />
        <MainContent />
      </div>
    </UserProvider>
  );
}

export default App;
```

**Usage in components:**
```jsx
import React from 'react';
import { useUser } from '../contexts/UserContext';

function Header() {
  const { currentUser, isAuthenticated, logout } = useUser();

  return (
    <header>
      {isAuthenticated ? (
        <div>
          <span>Welcome, {currentUser.name}!</span>
          <button onClick={logout}>Logout</button>
        </div>
      ) : (
        <span>Please log in</span>
      )}
    </header>
  );
}

export default Header;

// Another component using the same context
function ProfilePage() {
  const { currentUser } = useUser();

  return (
    <div>
      <h1>{currentUser?.name}'s Profile</h1>
      <p>Email: {currentUser?.email}</p>
    </div>
  );
}
```

---

## Essential React Libraries

| Category | Library | Purpose | WPF Equivalent |
|----------|---------|---------|----------------|
| **State Management** | Redux, Zustand, Jotai | Global state | Shared ViewModels, MVVM Light Messenger |
| **Routing** | React Router | Navigation | Frame, NavigationService |
| **Forms** | Formik, React Hook Form | Form handling | Binding, IDataErrorInfo |
| **HTTP Client** | Axios, Fetch API | API calls | HttpClient |
| **UI Components** | Material-UI, Ant Design, Chakra UI | Component library | MahApps, MaterialDesignInXaml |
| **Styling** | Styled Components, Emotion, Tailwind CSS | CSS-in-JS | Styles, Resources |
| **Testing** | Jest, React Testing Library | Unit testing | MSTest, xUnit, NUnit |
| **Data Fetching** | React Query, SWR | Server state management | N/A (manual implementation) |
| **Animation** | Framer Motion, React Spring | Animations | Storyboards, Animations |
| **Charts** | Recharts, Chart.js, Victory | Data visualization | LiveCharts, OxyPlot |

### Installation Examples:

```bash
# Routing
npm install react-router-dom

# State management
npm install redux react-redux @reduxjs/toolkit
npm install zustand

# Forms
npm install formik yup
npm install react-hook-form

# HTTP client
npm install axios

# UI libraries
npm install @mui/material @emotion/react @emotion/styled
npm install antd

# Styling
npm install styled-components
npm install -D tailwindcss

# Testing
npm install --save-dev @testing-library/react @testing-library/jest-dom

# Data fetching
npm install @tanstack/react-query

# Animation
npm install framer-motion
```

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
│   │       └── UserCard.xaml
│   ├── ViewModels/
│   │   ├── MainViewModel.cs
│   │   └── UserViewModel.cs
│   ├── Models/
│   │   └── User.cs
│   ├── Services/
│   │   └── UserService.cs
│   ├── Converters/
│   │   └── BoolToVisibilityConverter.cs
│   └── Resources/
│       └── Styles.xaml
└── WpfApp.Tests/
```

### React Project Structure:
```
react-app/
├── package.json
├── package-lock.json
├── public/
│   ├── index.html
│   └── favicon.ico
├── src/
│   ├── index.js
│   ├── App.js
│   ├── App.css
│   ├── components/
│   │   ├── Header/
│   │   │   ├── Header.jsx
│   │   │   └── Header.css
│   │   ├── UserCard/
│   │   │   ├── UserCard.jsx
│   │   │   └── UserCard.module.css
│   │   └── common/
│   │       └── Button.jsx
│   ├── pages/
│   │   ├── HomePage.jsx
│   │   ├── UserPage.jsx
│   │   └── AboutPage.jsx
│   ├── hooks/
│   │   ├── useFetch.js
│   │   └── useAuth.js
│   ├── contexts/
│   │   └── UserContext.js
│   ├── services/
│   │   └── userService.js
│   ├── utils/
│   │   ├── formatters.js
│   │   └── validators.js
│   ├── styles/
│   │   └── global.css
│   └── __tests__/
│       └── UserCard.test.js
└── node_modules/
```

---

## Common Patterns Translation

### WPF Pattern → React Equivalent

| WPF Pattern | React Pattern |
|-------------|---------------|
| **MVVM (Model-View-ViewModel)** | Component + Hooks (or Redux) |
| **DataContext** | Props / Context API |
| **Binding** | State + Props |
| **INotifyPropertyChanged** | useState / useReducer |
| **ICommand** | Event handlers (onClick, etc.) |
| **DependencyProperty** | Props with PropTypes |
| **Converter** | Helper functions |
| **DataTemplate** | Component composition |
| **Style** | CSS / CSS-in-JS |
| **Trigger** | Conditional rendering / className |
| **ObservableCollection** | State array (with immutable updates) |
| **RelayCommand** | Arrow functions |
| **Messenger/EventAggregator** | Context API / Redux |
| **Attached Properties** | Props / Context |

---

## Testing Comparison

### WPF Testing (MSTest/xUnit)

```csharp
[TestClass]
public class UserViewModelTests
{
    [TestMethod]
    public void IncrementCommand_IncrementsCount()
    {
        // Arrange
        var viewModel = new UserViewModel();
        var initialCount = viewModel.Count;

        // Act
        viewModel.IncrementCommand.Execute(null);

        // Assert
        Assert.AreEqual(initialCount + 1, viewModel.Count);
    }
}
```

### React Testing (Jest + React Testing Library)

```jsx
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

describe('Counter Component', () => {
  test('increments count when button is clicked', () => {
    // Arrange
    render(<Counter />);
    const button = screen.getByText(/increment/i);
    const count = screen.getByText(/count:/i);

    // Act
    fireEvent.click(button);

    // Assert
    expect(count).toHaveTextContent('Count: 1');
  });

  test('displays initial count of 0', () => {
    render(<Counter />);
    expect(screen.getByText(/count: 0/i)).toBeInTheDocument();
  });
});
```

---

## Best Practices

### 1. Component Design

**✅ DO:**
```jsx
// Small, focused components
function UserAvatar({ imageUrl, name }) {
  return (
    <img src={imageUrl} alt={name} className="avatar" />
  );
}

// Composition over inheritance
function UserCard({ user }) {
  return (
    <div className="user-card">
      <UserAvatar imageUrl={user.avatar} name={user.name} />
      <UserInfo name={user.name} email={user.email} />
    </div>
  );
}
```

**❌ DON'T:**
```jsx
// Large, monolithic components
function MassiveComponent() {
  // 500 lines of code...
  return <div>Too much logic here</div>;
}
```

### 2. State Management

**✅ DO:**
```jsx
// Keep state close to where it's used
function TodoList() {
  const [todos, setTodos] = useState([]);
  
  // State updates are immutable
  const addTodo = (text) => {
    setTodos([...todos, { id: Date.now(), text }]);
  };

  const removeTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };

  return (/* ... */);
}
```

**❌ DON'T:**
```jsx
// Mutate state directly (like WPF ObservableCollection)
const addTodo = (text) => {
  todos.push({ id: Date.now(), text }); // ❌ Wrong!
  setTodos(todos); // This won't trigger re-render
};
```

### 3. Props and PropTypes

**✅ DO:**
```jsx
import PropTypes from 'prop-types';

function Button({ label, onClick, variant = 'primary', disabled = false }) {
  return (
    <button 
      className={`btn btn-${variant}`}
      onClick={onClick}
      disabled={disabled}
    >
      {label}
    </button>
  );
}

Button.propTypes = {
  label: PropTypes.string.isRequired,
  onClick: PropTypes.func.isRequired,
  variant: PropTypes.oneOf(['primary', 'secondary', 'danger']),
  disabled: PropTypes.bool
};

export default Button;
```

### 4. useEffect Dependencies

**✅ DO:**
```jsx
useEffect(() => {
  fetchUserData(userId);
}, [userId]); // Include all dependencies
```

**❌ DON'T:**
```jsx
useEffect(() => {
  fetchUserData(userId);
}, []); // Missing dependency - causes stale closure
```

### 5. Key Prop for Lists

**✅ DO:**
```jsx
users.map(user => (
  <UserCard key={user.id} user={user} />
))
```

**❌ DON'T:**
```jsx
users.map((user, index) => (
  <UserCard key={index} user={user} /> // ❌ Don't use index as key
))
```

---

## TypeScript with React (Highly Recommended)

TypeScript brings C#-like type safety to React:

```tsx
// Define interfaces (like C# classes/interfaces)
interface User {
  id: number;
  name: string;
  email: string;
  isActive: boolean;
}

interface UserCardProps {
  user: User;
  onDelete: (id: number) => void;
}

// Typed component
const UserCard: React.FC<UserCardProps> = ({ user, onDelete }) => {
  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <button onClick={() => onDelete(user.id)}>Delete</button>
    </div>
  );
};

// Typed hooks
const [users, setUsers] = useState<User[]>([]);
const [count, setCount] = useState<number>(0);

// Generic types
function useFetch<T>(url: string): { 
  data: T | null; 
  isLoading: boolean; 
  error: Error | null 
} {
  const [data, setData] = useState<T | null>(null);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState<Error | null>(null);

  // ... implementation

  return { data, isLoading, error };
}

// Usage
const { data } = useFetch<User[]>('/api/users');
```

---

## Learning Path

### Week 1-2: Fundamentals
- ✅ Learn JavaScript/TypeScript basics
- ✅ Understand JSX syntax
- ✅ Master component creation
- ✅ Practice with props and state
- ✅ Learn basic event handling

### Week 3-4: Core Concepts
- ✅ Master hooks (useState, useEffect, useContext)
- ✅ Understand component lifecycle
- ✅ Learn conditional rendering
- ✅ Practice list rendering and keys
- ✅ Explore CSS styling approaches

### Week 5-6: Intermediate
- ✅ Implement React Router for navigation
- ✅ Learn form handling and validation
- ✅ Understand Context API for state
- ✅ Practice with custom hooks
- ✅ Learn async data fetching

### Week 7-8: Advanced
- ✅ Explore state management (Redux/Zustand)
- ✅ Learn performance optimization (useMemo, useCallback)
- ✅ Practice testing with Jest and React Testing Library
- ✅ Build a complete application
- ✅ Deploy to production (Vercel, Netlify)

---

## Tooling and Development

### WPF Tools:
- Visual Studio
- XAML Designer
- Blend
- ReSharper

### React Tools:
- **Code Editors**: VS Code (recommended), WebStorm
- **Browser DevTools**: React Developer Tools
- **Build Tools**: Create React App, Vite, Next.js
- **Package Manager**: npm, yarn, pnpm
- **Linting**: ESLint
- **Formatting**: Prettier

### Create React App (Quick Start):

```bash
# Create new React app
npx create-react-app my-app
cd my-app

# With TypeScript
npx create-react-app my-app --template typescript

# Start development server
npm start

# Build for production
npm run build
```

### Vite (Faster alternative):

```bash
# Create new Vite app
npm create vite@latest my-app -- --template react

# With TypeScript
npm create vite@latest my-app -- --template react-ts

cd my-app
npm install
npm run dev
```

---

## Key Mindset Shifts

### From WPF to React:

1. **Declarative vs Imperative**
   - WPF: Mix of declarative (XAML) and imperative (C# code-behind)
   - React: Fully declarative - describe what UI should look like

2. **Two-Way vs One-Way Binding**
   - WPF: Two-way binding is default
   - React: One-way data flow - explicit state updates

3. **XAML vs JSX**
   - WPF: XML-based markup
   - React: JavaScript with HTML-like syntax

4. **Classes vs Functions**
   - WPF: Class-based components (UserControls)
   - React: Functional components with hooks (modern approach)

5. **Styling**
   - WPF: Styles, templates, resources
   - React: CSS, CSS-in-JS, many approaches

6. **Immutability**
   - WPF: Mutable objects with notifications
   - React: Immutable state updates

---

## Resources

### Official Documentation:
- **React Docs**: https://react.dev/
- **React Router**: https://reactrouter.com/
- **Redux**: https://redux.js.org/

### Learning Platforms:
- **freeCodeCamp**: https://www.freecodecamp.org/
- **React Tutorial**: https://react.dev/learn
- **Scrimba**: https://scrimba.com/learn/learnreact

### Component Libraries:
- **Material-UI**: https://mui.com/
- **Ant Design**: https://ant.design/
- **Chakra UI**: https://chakra-ui.com/

### Tools:
- **React DevTools**: Browser extension for debugging
- **VS Code**: https://code.visualstudio.com/
- **Create React App**: https://create-react-app.dev/

---

## Conclusion

Transitioning from WPF to React requires learning new syntax and patterns, but many concepts translate well. Both frameworks focus on component-based UI development, state management, and data binding - just with different approaches.

### Key Takeaways:

✅ **Embrace functional programming** - React favors functions over classes  
✅ **Think in components** - Break UI into small, reusable pieces  
✅ **Master hooks** - They replace lifecycle methods and state management  
✅ **One-way data flow** - Explicitly manage state updates  
✅ **Immutability matters** - Always create new objects/arrays for state  
✅ **Learn modern JavaScript** - ES6+ features are essential  
✅ **Use TypeScript** - Brings C#-like type safety  
✅ **Practice, practice, practice** - Build projects to solidify concepts  

The web development ecosystem moves fast, but the fundamentals remain constant. Your WPF experience gives you a strong foundation in UI patterns and architecture that translates well to React!

---

**Happy coding with React! ⚛️**
