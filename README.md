IImports System.Runtime

Public Class Form1
    ' Объявление необходимых переменных
    Private _products As New List(Of Product)
    Private _suppliers As New List(Of Supplier)
    Private _orders As New List(Of Order)
    Private _warehouses As New List(Of Warehouse)
    Private _dataPanel As Panel
    Private _listBox As ListBox
    Private _mainPanel As Panel

    Private Sub Form1_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        ' Инициализация данных
        InitializeData()

        ' Создание и настройка элементов управления для отображения главного меню
        CreateMainMenu()

        ' Установка исходного размера формы
        Me.Size = New Size(600, 400)
    End Sub

    Private Sub CreateMainMenu()
        ' Создаем панель для размещения кнопок главного меню
        _mainPanel = New Panel With {.Dock = DockStyle.Top, .Height = 50}

        ' Создаем и добавляем кнопки на панель
        Dim btnProducts As Button = New Button With {.Text = "Продукты", .Dock = DockStyle.Left, .Width = 100}
        AddHandler btnProducts.Click, AddressOf BtnProducts_Click
        Dim btnSuppliers As Button = New Button With {.Text = "Поставщики", .Dock = DockStyle.Left, .Width = 100}
        AddHandler btnSuppliers.Click, AddressOf BtnSuppliers_Click
        Dim btnOrders As Button = New Button With {.Text = "Заказы", .Dock = DockStyle.Left, .Width = 100}
        AddHandler btnOrders.Click, AddressOf BtnOrders_Click
        Dim btnWarehouses As Button = New Button With {.Text = "Склады", .Dock = DockStyle.Left, .Width = 100}
        AddHandler btnWarehouses.Click, AddressOf BtnWarehouses_Click

        _mainPanel.Controls.Add(btnProducts)
        _mainPanel.Controls.Add(btnSuppliers)
        _mainPanel.Controls.Add(btnOrders)
        _mainPanel.Controls.Add(btnWarehouses)

        ' Добавляем панель на форму
        Me.Controls.Add(_mainPanel)
    End Sub

    Private Sub BtnProducts_Click(sender As Object, e As EventArgs)
        ' Отображение данных о продуктах
        Dim productForm As New ProductForm(_products)
        productForm.ShowDialog()
    End Sub

    Private Sub BtnSuppliers_Click(sender As Object, e As EventArgs)
        ' Отображение данных о поставщиках
        Dim supplierForm As New SupplierForm(_suppliers)
        supplierForm.ShowDialog()
    End Sub

    Private Sub BtnOrders_Click(sender As Object, e As EventArgs)
        ' Отображение данных о заказах
        Dim orderForm As New OrderForm(_orders)
        orderForm.ShowDialog()
    End Sub

    Private Sub BtnWarehouses_Click(sender As Object, e As EventArgs)
        ' Отображение данных о складах
        Dim warehouseForm As New WarehouseForm(_warehouses)
        warehouseForm.ShowDialog()
    End Sub

    Private Sub InitializeData()
        ' Инициализация данных для каждого списка
        _products.Add(New Product With {.Name = "Product 1"})
        _products.Add(New Product With {.Name = "Product 2"})
        _products.Add(New Product With {.Name = "Product 3"})
        _suppliers.Add(New Supplier With {.Name = "Supplier 1"})
        _suppliers.Add(New Supplier With {.Name = "Supplier 2"})
        _suppliers.Add(New Supplier With {.Name = "Supplier 3"})
        _orders.Add(New Order With {.OrderNumber = 1, .Customer = "Customer 1"})
        _orders.Add(New Order With {.OrderNumber = 2, .Customer = "Customer 2"})
        _orders.Add(New Order With {.OrderNumber = 3, .Customer = "Customer 3"})
        _warehouses.Add(New Warehouse With {.Name = "Warehouse 1"})
        _warehouses.Add(New Warehouse With {.Name = "Warehouse 2"})

        _warehouses.Add(New Warehouse With {.Name = "Warehouse 3"})
    End Sub
End Class

Public Class BaseForm
    Inherits Form
    Protected _listBox As ListBox
    Protected _textBox As TextBox
    Protected _btnAdd As Button
    Protected _btnDelete As Button
    Protected _btnBack As Button
    Protected _data As IList

    Public Sub New(data As IList)
        _data = data
        InitializeComponents()
        _listBox.DataSource = _data
        SetDisplayMember()
    End Sub

    Protected Overridable Sub SetDisplayMember()
        ' Устанавливает свойство DisplayMember для ListBox
        ' Реализация в производных классах
    End Sub

    Protected Sub InitializeComponents()
        ' Создание и настройка компонентов формы
        Me.Text = GetType(Me).Name.Replace("Form", "")
        Me.Size = New Size(600, 400)

        _listBox = New ListBox
        _listBox.Dock = DockStyle.Left
        _listBox.Width = 200

        _textBox = New TextBox
        _textBox.Dock = DockStyle.Top
        AddHandler _textBox.KeyPress, AddressOf _textBox_KeyPress

        _btnAdd = New Button
        _btnAdd.Text = "Добавить"
        _btnAdd.Dock = DockStyle.Top
        AddHandler _btnAdd.Click, AddressOf BtnAdd_Click

        _btnDelete = New Button
        _btnDelete.Text = "Удалить"
        _btnDelete.Dock = DockStyle.Top
        AddHandler _btnDelete.Click, AddressOf BtnDelete_Click

        _btnBack = New Button
        _btnBack.Text = "Назад"
        _btnBack.Dock = DockStyle.Top
        AddHandler _btnBack.Click, AddressOf BtnBack_Click

        Me.Controls.Add(_listBox)
        Me.Controls.Add(_textBox)
        Me.Controls.Add(_btnAdd)
        Me.Controls.Add(_btnDelete)
        Me.Controls.Add(_btnBack)
    End Sub

    Protected Sub _textBox_KeyPress(sender As Object, e As KeyPressEventArgs)
        ' Обработчик события KeyPress для текстового поля
        If e.KeyChar = Convert.ToChar(Keys.Enter) Then
            ' Когда пользователь нажимает Enter, добавляем новый элемент
            BtnAdd_Click(Me, EventArgs.Empty)
        End If
    End Sub

    Protected Overridable Sub BtnAdd_Click(sender As Object, e As EventArgs)
        ' Обработчик кнопки "Добавить"
        ' Реализация в производных классах
    End Sub

    Protected Sub BtnDelete_Click(sender As Object, e As EventArgs)
        ' Обработчик кнопки "Удалить"
        If _listBox.SelectedIndex >= 0 Then
            DeleteItem(_listBox.SelectedIndex)
            _listBox.DataSource = Nothing
            _listBox.DataSource = _data
        End If
    End Sub

    Protected Sub BtnBack_Click(sender As Object, e As EventArgs)
        ' Обработчик кнопки "Назад"
        Me.Close()
    End Sub

    Protected Overridable Sub DeleteItem(index As Integer)
        ' Удаляет элемент из списка
        ' Реализация в производных классах
    End Sub
End Class

Public Class ProductForm
    Inherits BaseForm

    Public Sub New(products As List(Of Product))
        MyBase.New(products)
    End Sub

    Protected Overrides Sub SetDisplayMember()
        _listBox.DisplayMember = "Name"
    End Sub

    Protected Overrides Sub BtnAdd_Click(sender As Object, e As EventArgs)
        ' Обработчик кнопки "Добавить"
        Dim newItem As New Product With {.Name = _textBox.Text}
        AddItem(newItem)
        _textBox.Clear()
    End Sub

    Private Sub AddItem(item As Product)
        CType(_data, List(Of Product)).Add(item)
    End Sub

    Protected Overrides Sub DeleteItem(index As Integer)
        CType(_data, List(Of Product)).RemoveAt(index)
    End Sub
End Class

Public Class SupplierForm
    Inherits BaseForm

    Public Sub New(suppliers As List(Of Supplier))

        MyBase.New(suppliers)
    End Sub

    Protected Overrides Sub SetDisplayMember()
        _listBox.DisplayMember = "Name"
    End Sub

    Protected Overrides Sub BtnAdd_Click(sender As Object, e As EventArgs)
        ' Обработчик кнопки "Добавить"
        Dim newItem As New Supplier With {.Name = _textBox.Text}
        AddItem(newItem)
        _textBox.Clear()
    End Sub

    Private Sub AddItem(item As Supplier)
        CType(_data, List(Of Supplier)).Add(item)
    End Sub

    Protected Overrides Sub DeleteItem(index As Integer)
        CType(_data, List(Of Supplier)).RemoveAt(index)
    End Sub
End Class

Public Class OrderForm
    Inherits BaseForm

    Public Sub New(orders As List(Of Order))
        MyBase.New(orders)
    End Sub

    Protected Overrides Sub SetDisplayMember()
        _listBox.DisplayMember = "OrderNumber"
    End Sub

    Protected Overrides Sub BtnAdd_Click(sender As Object, e As EventArgs)
        ' Обработчик кнопки "Добавить"
        Dim newOrder As New Order With {
            .OrderNumber = Integer.Parse(_textBox.Text),
            .Customer = ""
        }
        AddItem(newOrder)
        _textBox.Clear()
    End Sub

    Private Sub AddItem(item As Order)
        CType(_data, List(Of Order)).Add(item)
    End Sub

    Protected Overrides Sub DeleteItem(index As Integer)
        CType(_data, List(Of Order)).RemoveAt(index)
    End Sub
End Class

Public Class WarehouseForm
    Inherits BaseForm

    Public Sub New(warehouses As List(Of Warehouse))
        MyBase.New(warehouses)
    End Sub

    Protected Overrides Sub SetDisplayMember()
        _listBox.DisplayMember = "Name"
    End Sub

    Protected Overrides Sub BtnAdd_Click(sender As Object, e As EventArgs)
        ' Обработчик кнопки "Добавить"
        Dim newItem As New Warehouse With {.Name = _textBox.Text}
        AddItem(newItem)
        _textBox.Clear()
    End Sub

    Private Sub AddItem(item As Warehouse)
        CType(_data, List(Of Warehouse)).Add(item)
    End Sub

    Protected Overrides Sub DeleteItem(index As Integer)
        CType(_data, List(Of Warehouse)).RemoveAt(index)
    End Sub
End Class

Public Class Product
    Public Property Name As String
End Class

Public Class Supplier
    Public Property Name As String
End Class

Public Class Order
    Public Property OrderNumber As Integer
    Public Property Customer As String
End Class

Public Class Warehouse
    Public Property Name As String
End Class
