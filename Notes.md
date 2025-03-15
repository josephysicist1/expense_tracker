
- Expenses class'ının stateful widget olması lazım çünkü at some point, we wanna start managing expenses
we wanna start adding expenses, and that then will of course require a UI update, because everytime
we add a new expense, we wanna update the UI.
- models folderı altında data modellerini tanımlıyoruz. Burada widget yok, burada classlar ve propertyleri
var.

class Expense {
  final String id;
  final String title;
  final double amount;
  final DateTime date;
}

-  final DateTime date; expenselere harcanılan tarih de eklemek istediğmiz için DateTime değişken tipinde
bir değişken tanımladık.

class Expense {
  Expense({required this.title, required this.amount, required this.date});
  final String id;
  final String title;
  final double amount;
  final DateTime date;
} 
I dont wanna require id as a parameter, instead I wanna build such a unique ID dynamically whenever a new expense object is created. Bunu yapmak için 3. parti bir package kullanacagız, uuid package. Bu packageı generate unique ID işlemi için kullanıyoruz.

import 'package:uuid/uuid.dart';

final uuid = Uuid();

class Expense {
  Expense({required this.title, required this.amount, required this.date});
  final String id;
  final String title;
  final double amount;
  final DateTime date;
}

- In dart, Initializer Lists can be used to initialize class properties(like 'id'),with values that
are not received as constructor function arguments.

import 'package:uuid/uuid.dart';

final uuid = Uuid();

class Expense {
  Expense({required this.title, required this.amount, required this.date}) : id= uuid.v4();
  final String id;
  final String title;
  final double amount;
  final DateTime date;
}
Buradaki v4 metodu unique bir string id oluşturuyor. Expense class başlatıldıgında bu fonksiyon bir unique id oluşturup id değişkenine assign edecek.

- enum is a keyword that allows us to create custom type, you could say that could be named Category,
which simply is a combination of predefined allowed values.

import 'package:uuid/uuid.dart';

const uuid = Uuid();

enum Category { food, travel, leisure, work }

class Expense {
  Expense({required this.title, required this.amount, required this.date})
      : id = uuid.v4();
  final String id;
  final String title;
  final double amount;
  final DateTime date;
  final Category category;
}

import 'package:uuid/uuid.dart';

const uuid = Uuid();

enum Category { food, travel, leisure, work }

class Expense {
  Expense({
    required this.title,
    required this.amount,
    required this.date,
    required this.category,
  }) : id = uuid.v4();
  final String id;
  final String title;
  final double amount;
  final DateTime date;
  final Category category;
}

- Whenever you create a class, you automatically also get a type of the same name
  final List<Expense> _registeredExpenses = [];

-  final List<Expense> _registeredExpenses = [
    Expense(
        title: 'Flutter Course',
        amount: 19.99,
        date: DateTime.now(),
        category: Category.work)
  ];
  

- 
import 'package:expense_tracker/models/expense.dart';
import 'package:flutter/material.dart';

class ExpensesList extends StatelessWidget {
  const ExpensesList({super.key, required this.expenses});

  final List<Expense> expenses;

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [],
    );
  }
}

- 
import 'package:expense_tracker/models/expense.dart';
import 'package:flutter/material.dart';

class ExpensesList extends StatelessWidget {
  const ExpensesList({super.key, required this.expenses});

  final List<Expense> expenses;

  @override
  Widget build(BuildContext context) {
    return ListView(
      children: [],
    );
  }
}
ListView kullanman durumunda scrollable list'in olacak.

-    return ListView.builder(itemBuilder: itemBuilder)
this is a special constructor function for this listview widget, which in the end tells the Flutter
that it should create a scrollable list. ListViews are always scrollable list by default, but it should build, create those list items only when they are visible or about to become visible not if they are not visible.

- return ListView.builder(itemBuilder: (ctx, index) {
      return Text('');
    });
yukarıdaki satırla aşağıdaki satır aynı
    return ListView.builder(itemBuilder: (ctx, index) => Text(''));
itemBuilder bir fonksiyonu parametre olarak alıyor ve bu parametre olarak aldıgı fonksiyon
bir Text widget döndürüyor.

-    return ListView.builder(itemCount: expenses.length,itemBuilder: (ctx, index) => Text(''));
itemCount in the end defines how many list items will be rendered eventually.

-  return ListView.builder(
        itemCount: expenses.length,
        itemBuilder: (ctx, index) => Text(expenses[index].title));
Burada expenses listesinin lengthi kadar çağrılacak ve her çağrıldığında index otomatik olarak artacak,
bundan dolayı expenses listini sırasıyla çağırmış olacak.

- 
import 'package:expense_tracker/expenses_list.dart';
import 'package:flutter/material.dart';
import 'package:expense_tracker/models/expense.dart';

class Expenses extends StatefulWidget {
  const Expenses({super.key});

  @override
  State<Expenses> createState() => _ExpensesState();
}

class _ExpensesState extends State<Expenses> {
  final List<Expense> _registeredExpenses = [
    Expense(
        title: 'Flutter Course',
        amount: 19.99,
        date: DateTime.now(),
        category: Category.work),
    Expense(
        title: 'Cinema',
        amount: 40.00,
        date: DateTime.now(),
        category: Category.leisure)
  ];
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: [
          const Text('the chart'),
          ExpensesList(expenses: _registeredExpenses),
        ],
      ),
    );
  }
}

-     return Card();
Card() styling amacıyla kullanılıyor because this will give us a nice card look. It will basically
put the content that we pass to card into a container that is a bit elevated, that has a slight
shadow behind itself to make it stand out a bit more

-                  expense.amount.toStringAsFixed(2),
12.3433 'ü 12.34 olarak yazıyor. Virgülden sonra 2 digit'a fixliyor.

-  Text('${expense.amount.toStringAsFixed(2)}',)

-Row(
    children: [
        Text('\$${expense.amount.toStringAsFixed(2)}'),
        Spacer(),
        Row(),
    ],
),
Spacer() is a widget that can be used in any columns or rows, to tell Flutter that it should
create a widget that takes up all the space it can get between the other widget between which 
it is placed. Yukarıdaki koda bakacak olursak Text() widget takes all the space it needs to
output the text and alttaki Row() will get the all space it needs to output its child items, but
spacer then will always take all the remaining space hence pushing text to the left and this to the right. 

- 
import 'package:flutter/material.dart';
import 'package:uuid/uuid.dart';

const uuid = Uuid();

enum Category { food, travel, leisure, work }

const categoryIcons = {
  Category.food: Icons.lunch_dining,
};

class Expense {
  Expense({
    required this.title,
    required this.amount,
    required this.date,
    required this.category,
  }) : id = uuid.v4();
  final String id;
  final String title;
  final double amount;
  final DateTime date;
  final Category category;
}

- 
import 'package:flutter/material.dart';
import 'package:uuid/uuid.dart';

const uuid = Uuid();

enum Category { food, travel, leisure, work }

const categoryIcons = {
  Category.food: Icons.lunch_dining,
  Category.travel: Icons.flight_takeoff,
  Category.leisure: Icons.movie,
  Category.work: Icons.work
};

class Expense {
  Expense({
    required this.title,
    required this.amount,
    required this.date,
    required this.category,
  }) : id = uuid.v4();
  final String id;
  final String title;
  final double amount;
  final DateTime date;
  final Category category;
}

- Getters are basically "computed properties" => Properties that are dynamically derived,
based on other class properties.
  String get formattedDate {
    return 
  }


- Flutter'ın intl package'ı datelerin formatını manage etmemizde işe yarıyor.

final formatter = DateFormat.yMd();

- 
import 'package:flutter/material.dart';
import 'package:uuid/uuid.dart';
import 'package:intl/intl.dart';

const uuid = Uuid();
final formatter = DateFormat.yMd();

enum Category { food, travel, leisure, work }

const categoryIcons = {
  Category.food: Icons.lunch_dining,
  Category.travel: Icons.flight_takeoff,
  Category.leisure: Icons.movie,
  Category.work: Icons.work
};

class Expense {
  Expense({
    required this.title,
    required this.amount,
    required this.date,
    required this.category,
  }) : id = uuid.v4();
  final String id;
  final String title;
  final double amount;
  final DateTime date;
  final Category category;

  String get formattedDate {
    return formatter.format(date);
  }
}

- 
import 'package:expense_tracker/models/expense.dart';
import 'package:flutter/material.dart';

class ExpenseItem extends StatelessWidget {
  const ExpenseItem({super.key, required this.expense});
  final Expense expense;

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 16),
        child: Column(
          children: [
            Text(expense.title),
            const SizedBox(height: 4),
            Row(
              children: [
                Text('\$${expense.amount.toStringAsFixed(2)}'),
                Spacer(),
                Row(
                  children: [
                    Icon(categoryIcons[expense.category]),
                    SizedBox(width: 8),
                    Text(expense.formattedDate)
                  ],
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}

- return Scaffold(
      appBar: AppBar(
        actions: [],
      ), ...
It offers an action parameter, which we can set. Actions wants a list of widgets and this is
typically used to display buttons in this top app bar. Bunun içine istediğin butonları koyabilirsin. 

-  void _openAddExpenseOverlay() {
    showModalBottomSheet(context: context, builder: builder)
  } 
  buradaki showModal.. function dynamically add a new UI element such as modal overlay when
  it is being executed. showModalBottomSheet(context: context, builder: builder) buradaki context
  object , you can think of this context object, this context value is some kind of metadata 
  collection an object full of metadata managed by flutter to a specific widget. So every widget has its own context object and that contains metadata information related to the widget and 
  very important related to the widget's position in the overall UI, in the over all widget tree.
  Ne zaman builder argument'i görsen senden fonksiyon istiyor demektir. Burada builder açıklamasında {required Widget Function(BuildContext)} yazıyor, yani bizim sağlayacağımız fonksiyon bir widget döndürmeli.

  void _openAddExpenseOverlay() {
    showModalBottomSheet(
        context: context, builder: (ctx) => Text('Modal Bottom Sheeet'));
  }

- Şimdi new_expense.dart dosyası oluşturarak, yukarıdaki _openAdd.. fonksiyonunun içindeki showModalBottomSheet fonksiyonuna göndereceğiz. Buradaki builder'a bunu verdikten sonra +'ya tıklaması durumunda bir sayfa aşağıdan yukarı acılacak ve title, expense, date gibi verileri girdikten sonra ekleyecek.

 child: Column(
          children: [
            TextField(),
          ],
TextField Widget'ı kullanıcıların text girmesine olanak tanıyor. Bunun propertylerinden biri de
maxLength, bu propery kullanıcıya en fazla kaç karakter gireceğinin belirtilmesini sağlıyor
 TextField(
              maxLength: 15,
              keyboardType: TextInputType.name,
            ),
buradaki keyboardtype, aşağıdan cıkan klavyenin başlangıç ekranının mail yazmaya mı, yoksa telefon numarası girmeye mi ayarlanması gerektiğini gösteriyor. Diğer bir propery de decoration. TextField'ın ne için oldugunu kullanıcıya söylemek için bir label gerekiyor, bunu da decoration ile yapıyoruz.

- 
import 'package:expense_tracker/widgets/expenses_list/expenses_list.dart';
import 'package:expense_tracker/widgets/new_expense.dart';
import 'package:flutter/material.dart';
import 'package:expense_tracker/models/expense.dart';

class Expenses extends StatefulWidget {
  const Expenses({super.key});

  @override
  State<Expenses> createState() => _ExpensesState();
}

class _ExpensesState extends State<Expenses> {
  final List<Expense> _registeredExpenses = [
    Expense(
        title: 'Flutter Course',
        amount: 19.99,
        date: DateTime.now(),
        category: Category.work),
    Expense(
        title: 'Cinema',
        amount: 40.00,
        date: DateTime.now(),
        category: Category.leisure)
  ];

  void _openAddExpenseOverlay() {
    showModalBottomSheet(context: context, builder: (ctx) => NewExpense());
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Flutter Expense Tracker'),
        actions: [
          IconButton(
            onPressed: _openAddExpenseOverlay,
            icon: Icon(Icons.add),
          )
        ],
      ),
      body: Column(
        children: [
          const Text('the chart'),
          Expanded(child: ExpensesList(expenses: _registeredExpenses)),
        ],
      ),
    );
  }
}

- 
import 'package:flutter/material.dart';

class NewExpense extends StatefulWidget {
  const NewExpense({super.key});

  @override
  State<NewExpense> createState() => _NewExpenseState();
}

class _NewExpenseState extends State<NewExpense> {
  @override
  Widget build(BuildContext context) {
    return Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              maxLength: 15,
              decoration: InputDecoration(
                label: Text('Title'),
              ),
            ),
          ],
        ));
  }
}

- 
import 'package:flutter/material.dart';

class NewExpense extends StatefulWidget {
  const NewExpense({super.key});

  @override
  State<NewExpense> createState() => _NewExpenseState();
}

class _NewExpenseState extends State<NewExpense> {
  var _enteredTitle = '';
  void _saveTitleInput(String inputValue) {
    _enteredTitle = inputValue;
  }

  @override
  Widget build(BuildContext context) {
    return Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              onChanged: _saveTitleInput,
              maxLength: 15,
              decoration: const InputDecoration(
                label: Text('Title'),
              ),
            ),
          ],
        ));
  }
}
onChanged bizden string bir değişken alan void fonksiyon istedi, biz de yukarıda tanımladık ve onChanged'e atadık.

-  final _titleController = TextEditingController();
when you create a text editing controller,you also have to tell flutter to delete that controller
when the widget is not needed anymore çünkü diğer durumda bu widget sen bottom menuyu kapatsan bile memoryde kalmaya devam edecek bu da performansı etkileyecek. Bundan dolayı dispose metodu ekle. dispose, initstate ve build gibi stateful widgetın bir parçası. It's called automatically by flutter when the widget and its state are about to be destroyed

-  final _titleController = TextEditingController();

  @override
  void dispose() {
    _titleController.dispose();
    super.dispose();
  }

- 
import 'package:flutter/material.dart';

class NewExpense extends StatefulWidget {
  const NewExpense({super.key});

  @override
  State<NewExpense> createState() => _NewExpenseState();
}

class _NewExpenseState extends State<NewExpense> {
  final _titleController = TextEditingController();
  final _amountController = TextEditingController();

  @override
  void dispose() {
    _amountController.dispose();
    _titleController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _titleController,
              maxLength: 15,
              decoration: const InputDecoration(
                label: Text('Title'),
              ),
            ),
            TextField(
              controller: _titleController,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                label: Text('Amount'),
              ),
            ),
            Row(
              children: [
                ElevatedButton(
                    onPressed: () {
                      print(_titleController.text);
                    },
                    child: const Text('Save expense'))
              ],
            ),
          ],
        ));
  }
}

- print(_amountController.text); 
Number bile girilse string olarak bastırıyor.

- 
 TextField(
              controller: _amountController,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                prefixText: '\$',
                label: Text('Amount'),
              ),
            ),

- 
 TextButton(
                    onPressed: () {
                      Navigator.pop(context);
                    },
                    child: Text('Cancel')),
Bu buton, içindeki Navigator class'ının pop metodu sayesinde aşağıda açılan showModalBottomSheet'i 
kapatıyor.

- 
import 'package:flutter/material.dart';

class NewExpense extends StatefulWidget {
  const NewExpense({super.key});

  @override
  State<NewExpense> createState() => _NewExpenseState();
}

class _NewExpenseState extends State<NewExpense> {
  final _titleController = TextEditingController();
  final _amountController = TextEditingController();

  @override
  void dispose() {
    _amountController.dispose();
    _titleController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _titleController,
              maxLength: 15,
              decoration: const InputDecoration(
                label: Text('Title'),
              ),
            ),
            TextField(
              controller: _amountController,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                prefixText:
                    '\$ ', //amount önünde her zaman dolar işareti olacak
                label: Text('Amount'),
              ),
            ),
            Row(
              children: [
                TextButton(
                    onPressed: () {
                      Navigator.pop(context);
                    },
                    child: Text('Cancel')),
                ElevatedButton(
                    onPressed: () {
                      print(_titleController.text);
                      print(_amountController.text);
                    },
                    child: const Text('Save expense'))
              ],
            ),
          ],
        ));
  }
}
- 
child: Row(
                    children: [
                      Text('Select a date'),
                      IconButton(onPressed: onPressed, icon: icon)
                    ]
IconButton'ı datePicker'ı açması için koyduk buraya.

- 
 void _presentDatePicker() {
    final now = DateTime.now();
    final firstDate = DateTime(now.year - 1, now.month, now.day);
    showDatePicker(
        context: context,
        initialDate: now,
        firstDate: firstDate,
        lastDate: now);
  }

- Future is an object that wraps a value which you dont have yet but you will have in the future

- showDatePicker(
        context: context,
        initialDate: now,
        firstDate: firstDate,
        lastDate: now).then((value){});
  } shoDatePicker'ın then metodu var ve bu metoda fonksiyon gönderebiliyorsun, mesela anonim fonksiyon, which "then" will get that value, which in this case here has been picked once it has
  been picked, so this function which you pass to "then"  will be executed by flutter once the value is available, so once the date has been picked here.

- Text(_selectedDate == null
                          ? 'No date selected'
                          : formatter.format(_selectedDate!)),
  burada normalde hata alıyorduk çünkü format fonksiyonu null içerikli bir şey olamaz, ancak sonuna ! koyarak bunu aştık.
  ! ifade dart'ı selectedDate'in null olmayacagını söylüyor

  - 

  import 'package:flutter/material.dart';
import 'package:expense_tracker/models/expense.dart';

class NewExpense extends StatefulWidget {
  const NewExpense({super.key});

  @override
  State<NewExpense> createState() => _NewExpenseState();
}

class _NewExpenseState extends State<NewExpense> {
  final _titleController = TextEditingController();
  final _amountController = TextEditingController();
  DateTime? _selectedDate;

  void _presentDatePicker() async {
    final now = DateTime.now();
    final firstDate = DateTime(now.year - 1, now.month, now.day);
    final pickedDate = await showDatePicker(
        context: context,
        initialDate: now,
        firstDate: firstDate,
        lastDate: now);
    setState(() {
      _selectedDate = pickedDate;
    });
  }

  @override
  void dispose() {
    _amountController.dispose();
    _titleController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _titleController,
              maxLength: 15,
              decoration: const InputDecoration(
                label: Text('Title'),
              ),
            ),
            Row(
              children: [
                Expanded(
                  child: TextField(
                    controller: _amountController,
                    keyboardType: TextInputType.number,
                    decoration: const InputDecoration(
                      prefixText:
                          '\$ ', //amount önünde her zaman dolar işareti olacak
                      label: Text('Amount'),
                    ),
                  ),
                ),
                SizedBox(
                  width: 16,
                ),
                Expanded(
                  child: Row(
                    mainAxisAlignment: MainAxisAlignment.end,
                    crossAxisAlignment: CrossAxisAlignment.center,
                    children: [
                      Text(_selectedDate == null
                          ? 'No date selected'
                          : formatter.format(_selectedDate!)),
                      IconButton(
                          onPressed: _presentDatePicker,
                          icon: Icon(Icons.calendar_month))
                    ],
                  ),
                ),
              ],
            ),
            Row(
              children: [
                TextButton(
                    onPressed: () {
                      Navigator.pop(context);
                    },
                    child: Text('Cancel')),
                ElevatedButton(
                    onPressed: () {
                      print(_titleController.text);
                      print(_amountController.text);
                    },
                    child: const Text('Save expense'))
              ],
            ),
          ],
        ));
  }
}

-  DropdownButton(items: items, onChanged: () {}),
onChanged kısmında bu butona basıldıgında hangi fonksiyon hayata geçirelecek onu gösteriyor. onChanged fonksiyonu bir
değer alıyor, ve bu değer dynamic bir değer. onChanged : (value){} The value that it will get here will be the value
that has been selected from the dropdown and it's of type dynamic

- 
DropdownButton(
                    items: Category.values
                        .map(
                          (category) => DropdownMenuItem(
                            value: category,
                            child: Text(category.name),
                          ),
                        )
                        .toList(),
                    onChanged: (value) {}),

- 
DropdownButton(
                    items: Category.values
                        .map(
                          (category) => DropdownMenuItem(
                            value: category,
                            child: Text(category.name.toUpperCase()),
                          ),
                        )
                        .toList(),
                    onChanged: (value) {
                      if (value == null) {
                        return;
                      }
                      setState(() {
                        _selectedCategory = value;
                      });
                    }),
Burada onChanged fonksiyonunda eğer if kullanmazsak hata alıyoruz çünkü value null olabilir. Eğer null ise return kullanıp
onChanged fonksiyonundan cıktık. Return'den sonraki kod bloğu artık çalışmayacak. 

-     if (_titleController.text.trim()) trim removes excess white space at the beginning or end. trim() metodu, bir String’in başındaki ve sonundaki boşlukları (whitespace) kaldırır.

-     final _enteredAmount = double.tryParse(_amountController.text);
double.tryParse() metodu içine string alır ve eğer double'a çevrilebiliyorsa double değer döndürür
eğer çevrilemiyorsa içindeki değer o zaman null değer döndürür. tryParse('hello') null değer döndürür 

-     final amountisValid = _enteredAmount == null;
amountisValid değişkenine bool bir değer assign edilecek ve değer assign edilmesi durumu enteredamount'un null olup
olmamasına bağlı. 

-     final amountisValid = _enteredAmount == null || _enteredAmount <= 0;

- 
void _submitExpenseData() {
    final _enteredAmount = double.tryParse(_amountController.text);
    final amountisValid = _enteredAmount == null || _enteredAmount <= 0;
    if (_titleController.text.trim().isEmpty ||
        amountisValid ||
        _selectedDate == null) {
      // show error message
    }
  }

-       showDialog(context: context, builder: builder)
ekranda info ya da error göstermek için kullandığımız bir fonksiyon. 
showDialog(
  context: context,
  builder: (BuildContext context) {
    return AlertDialog(
      title: Text("Uyarı"),
      content: Text("Bu bir dialog penceresidir."),
      actions: [
        TextButton(
          onPressed: () {
            Navigator.of(context).pop(); // Dialog'u kapat
          },
          child: Text("Tamam"),
        ),
      ],
    );
  },
);

- 
showDialog(
        context: context,
        builder: (ctx) => AlertDialog(
          title: Text('Invalid Input'),
          content: Text(
              'make sure a valid title, date and amount and category is entered'),
          actions: [
            TextButton(
                onPressed: () {
                  Navigator.pop(ctx);
                },
                child: Text('okay'))
          ],
        ),

- 
 void _addExpense(Expense expense) {
    setState(() {
      _registeredExpenses.add(expense);
    });
  }
  setState içinde olmalı ki, state'i güncelleyip sonuçlarını ekranda görebilelim.

- 

import 'package:expense_tracker/widgets/expenses_list/expenses_list.dart';
import 'package:expense_tracker/widgets/new_expense.dart';
import 'package:flutter/material.dart';
import 'package:expense_tracker/models/expense.dart';

class Expenses extends StatefulWidget {
  const Expenses({super.key});

  @override
  State<Expenses> createState() => _ExpensesState();
}

class _ExpensesState extends State<Expenses> {
  final List<Expense> _registeredExpenses = [
    Expense(
        title: 'Flutter Course',
        amount: 19.99,
        date: DateTime.now(),
        category: Category.work),
    Expense(
        title: 'Cinema',
        amount: 40.00,
        date: DateTime.now(),
        category: Category.leisure)
  ];

  void _openAddExpenseOverlay() {
    showModalBottomSheet(
        context: context,
        builder: (ctx) => NewExpense(onAddExpense: _addExpense));
  }

  void _addExpense(Expense expense) {
    setState(() {
      _registeredExpenses.add(expense);
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Flutter Expense Tracker'),
        actions: [
          IconButton(
            onPressed: _openAddExpenseOverlay,
            icon: Icon(Icons.add),
          )
        ],
      ),
      body: Column(
        children: [
          const Text('the chart'),
          Expanded(child: ExpensesList(expenses: _registeredExpenses)),
        ],
      ),
    );
  }
}

- 
import 'package:flutter/material.dart';
import 'package:expense_tracker/models/expense.dart';

class NewExpense extends StatefulWidget {
  const NewExpense({super.key, required this.onAddExpense});
  final void Function(Expense expense) onAddExpense;

  @override
  State<NewExpense> createState() => _NewExpenseState();
}

class _NewExpenseState extends State<NewExpense> {
  final _titleController = TextEditingController();
  final _amountController = TextEditingController();
  DateTime? _selectedDate;
  Category _selectedCategory = Category.leisure;

  void _presentDatePicker() async {
    final now = DateTime.now();
    final firstDate = DateTime(now.year - 1, now.month, now.day);
    final pickedDate = await showDatePicker(
        context: context,
        initialDate: now,
        firstDate: firstDate,
        lastDate: now);
    setState(() {
      _selectedDate = pickedDate;
    });
  }

  void _submitExpenseData() {
    final _enteredAmount = double.tryParse(_amountController.text);
    final amountisValid = _enteredAmount == null || _enteredAmount <= 0;
    if (_titleController.text.trim().isEmpty ||
        amountisValid ||
        _selectedDate == null) {
      showDialog(
        context: context,
        builder: (ctx) => AlertDialog(
          title: Text('Invalid Input'),
          content: Text(
              'make sure a valid title, date and amount and category is entered'),
          actions: [
            TextButton(
                onPressed: () {
                  Navigator.pop(ctx);
                },
                child: Text('okay'))
          ],
        ),
      );
      return;
    }
    widget.onAddExpense(Expense(
        title: _titleController.text,
        amount: _enteredAmount,
        date: _selectedDate!,
        category: _selectedCategory));
  }

  @override
  void dispose() {
    _amountController.dispose();
    _titleController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _titleController,
              maxLength: 15,
              decoration: const InputDecoration(
                label: Text('Title'),
              ),
            ),
            Row(
              children: [
                Expanded(
                  child: TextField(
                    controller: _amountController,
                    keyboardType: TextInputType.number,
                    decoration: const InputDecoration(
                      prefixText:
                          '\$ ', //amount önünde her zaman dolar işareti olacak
                      label: Text('Amount'),
                    ),
                  ),
                ),
                SizedBox(
                  width: 16,
                ),
                Expanded(
                  child: Row(
                    mainAxisAlignment: MainAxisAlignment.end,
                    crossAxisAlignment: CrossAxisAlignment.center,
                    children: [
                      Text(_selectedDate == null
                          ? 'No date selected'
                          : formatter.format(_selectedDate!)),
                      IconButton(
                          onPressed: _presentDatePicker,
                          icon: Icon(Icons.calendar_month))
                    ],
                  ),
                ),
              ],
            ),
            const SizedBox(
              height: 16.0,
            ),
            Row(
              children: [
                DropdownButton(
                    value: _selectedCategory,
                    items: Category.values
                        .map(
                          (category) => DropdownMenuItem(
                            value: category,
                            child: Text(category.name.toUpperCase()),
                          ),
                        )
                        .toList(),
                    onChanged: (value) {
                      if (value == null) {
                        return;
                      }
                      setState(() {
                        _selectedCategory = value;
                      });
                    }),
                const Spacer(),
                TextButton(
                    onPressed: () {
                      Navigator.pop(context);
                    },
                    child: Text('Cancel')),
                ElevatedButton(
                    onPressed: _submitExpenseData,
                    child: const Text('Save expense'))
              ],
            ),
          ],
        ));
  }
}

-  showModalBottomSheet(
        isScrollControlled: true,
        context: context,
        builder: (ctx) => NewExpense(onAddExpense: _addExpense));
true yaptıgın özellik sayesinde keyboardla bir çakışma olmayacak ancak ekran tamamen kaplanmış oluyor.

- Dismissible(
        key: key,
        child: ExpenseItem(expense: expenses[index]),
      ),
Dismissible'da key olmak zorunda çünkü doğru elemanın silinip silinmediğinden emin olmak lazım.

- 
ListView.builder(
      itemCount: expenses.length,
      itemBuilder: (ctx, index) => Dismissible(
        key: ValueKey(expenses[index]),
        child: ExpenseItem(expense: expenses[index]),
      ),
    );

-   @override
  Widget build(BuildContext context) {
    Widget mainContent = Center(child: Text('No Expenses Found. Start adding'));
    return Scaffold( ...

-  ScaffoldMessenger.of(context)
        .showSnackBar(SnackBar(content: Text('Expense Deleted')));

- ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        duration: Duration(seconds: 3),
        content: Text('Expense Deleted'),
        action: SnackBarAction(label: 'Undo', onPressed: () {}),
      ),
    );
    action kısmında eğer "undo" butonuna basılırsa onPressed çağrılacak.


- 
 void _removeExpense(Expense expense) {
    setState(() {
      _registeredExpenses.remove(expense);
    });
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        duration: Duration(seconds: 3),
        content: Text('Expense Deleted'),
        action: SnackBarAction(label: 'Undo', onPressed: () {}),
      ),
    );
  }

- 
void _removeExpense(Expense expense) {
    final expenseIndex = _registeredExpenses.indexOf(expense);
    setState(() {
      _registeredExpenses.remove(expense);
    });
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        duration: Duration(seconds: 3),
        content: Text('Expense Deleted'),
        action: SnackBarAction(
            label: 'Undo',
            onPressed: () {
              setState(() {
                _registeredExpenses
                    .insert(expenseIndex,expense); // cıkardıgını tekrar ekliyorsun
              });
            }),
      ),
    );
  }
  burada undo butonuna basınca cıkardıgımız indextekini, aynı indexe tekrar ekliyor. Add fonksiyonu da kullanabilirdik 
  ancak o zaman cıkardıgımız yere ekleyemezdik.

  