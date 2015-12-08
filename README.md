# CoffeeScript Style Guide

#### Кодировка
* Все стили продукта должны использовать кодировку UTF-8
* Все исходные коды должны быть описаны только с использованием латиницы (включая комментарии)

#### Ширина строки
Разрешенная максимальная длина строки в файлах равно 80 символам. Делая строчки не большими вы облегчаете ревью кода, а также сравнение двух файлов в IDE. 

#### Пустые строки
  * Необходимо оставлять последнюю строчку файла пустой, лучше всего настроить IDE 
  * Необходимо отделять пустой строкой:
    - определения функций и методов класса
    - логические участки кода внутри функций и методов
    - функции при описании свойств объекта
    - объекты внутри массивов, таким образом чтобы `,` находилась после пустой строки
  * Необходимо отделять 2-мя пустыми строками:
    - определения классов

#### Использование strict режима
Каждый файл исходного кода должен начинаться с "use strict"

#### Исключения (exceptions)
  * Запрещается подавлять исключения в коде

```coffeescript
# -------- BAD ----------

try
  null.toString()
  
# -------- OK -----------

try
  null.toString()
catch e
  sendErrorToServer e.message, e.stack
  throw e
```

#### Парадигма программирования
  * Код должен быть написан в императивном стиле
  * Запрещено смешивать парадигмы в рамках одного продукта

```coffeescript
# -------- GOOD ---------

tasks = common_tasks.concat [
  "concat:locale_RU"
  "concat:locale_EN"
  "sass:dist"
]
.concat [
  target is "watch" and target or []
]
 
grunt.task.run tasks

# -------- BAD ----------

grunt.task.run if 1
  common_tasks
  .concat [
     "concat:locale_RU"
     "concat:locale_EN"
     "sass:dist"
  ]
  .concat if target is "watch"
    "watch"
  else
    []
```

#### Обработка данных
  * Следует использовать библиотеки для обработки данных, такие как lodash, и расширять их возможности
  * Категорически запрещено вносить изменения в нативные объекты языка Javascript

```coffeescript
# -------- GOOD ---------

_.mixin capitalize: (string) -> # ...

_.capitalize String(obj)

isArray = $.type null # null

# -------- BAD ----------

String.prototype.capitalize = -> # ...

Object::toString.call(obj).capitalize()

isArray = typeof null # object
```

#### Отступы
  * Во всех файлах должен использоваться отступ  в два пробела
  * Для простого поддержания данного правила необходимо настроить используемое IDE
  * При описании элементов объекта (Object) или массива (Array) отступ перед закрывающей скобкой должен соответствовать отступу перед выражением, содержащим открывающую скобку

```coffeescript 
# -------- GOOD ---------

foo = [
  'some'
  'string'
  'values'
]

# -------- BAD ----------

foo = [
    'some'
    'string'
    'values'
    ]
```

  * В блоке условий отступ для всех условий, описанных с новой строки должен:
    - быть увеличен на 4 пробела, при этом первое условие, стоящее после `if` должно быть выравнено пробелами
    - таким же как и в строке с `if`

```coffeescript 
# -------- GOOD ---------

if  not options.force and 
    not mix.consistent
  @stop()
  
# --------- OK ----------

if not options.force and 
not mix.consistent
  @stop()

# -------- BAD ----------

if not options.force and 
 not mix.consistent
  @stop()
```

  * При описании цепочки вызовов (call chain), необходимо сохранять отступ

```coffeescript 
# -------- GOOD ---------

_(arr).map arr, (el) ->
    return a if el.is("a")
.compact()
.value()

# -------- BAD ----------

_(arr).map(arr, (el) ->
  return a if el.is("a")
  )
  .compact()
  .value()
```

#### Запятые
  * Избегайте использования запятых до символа новой строки, при описании элементов объекта (Object) или массива (Array), когда элементы перечислены в отдельных строках. 
  * Используйте запятую вместе с пустой строкой чтобы отделять объекты в массиве друг от друга

```coffeescript 
# -------- GOOD ---------

foo = [
  field : "name"
  name  : $.i18n.t 'settings.services.name'
  
  formatter: (row, cell, value, columnDef, dataContext) ->
    # ...
  
, 
  field : "name"
  name  : $.i18n.t 'settings.services.name'
  
  formatter: (row, cell, value, columnDef, dataContext) ->
    # ...
]

bar =
  label : 'test'
  value : 87

# -------- BAD ----------

foo = [
    field     : "name",
    name      : $.i18n.t 'settings.services.name',
    formatter : (row, cell, value, columnDef, dataContext) ->
      # ...
  , 
    field     : "name",
    name      : $.i18n.t 'settings.services.name',
    formatter : (row, cell, value, columnDef, dataContext) ->
      # ...
  ]
bar =
  label: 'test',
  value: 87

```

#### Пробелы
  * Не используйте дополнительные пробелы:
    - перед скобками и после них
    - перед запятыми
    - перед двоеточиями, если не используете выравнивание (alignment)
    - в конце строки (trailing spaces)
    
  * Используйте 1 пробел (c учетом вышеописанного):
    - после
      - запятых
      - двоеточия, если не используете выравнивание (aligment)
    - если не используете выравнивание (aligment), перед и после:
      - операторов присваивания `=`
      - операторов дополнения значений `-=`, `or=`, `+=`, etc
      - операторов сравнения `>=`, `<`, etc
      - арефметических операторов `*`, `+`, `-`, `/`, etc
      - симолвами `->` и `=>`
  

```coffeescript 
# -------- GOOD ---------

a = [1, 2, 3, 4]
b = {foo: true}
c = ($ 'body')
  
console.log x, y
 
test: (param = null) ->
  return
  
x +=  1
y or= true

# -------- BAD ----------

a = [ 1, 2, 3, 4 ]
b = { foo : true }
c = ( $ 'body' )
  
console.log x , y
 
test: ( param= null ) ->
  return
  
x += 1
y or= true

```

  * Необходимо выравнивать символы `=` `:` при многострочном описании блока присваивания или объекта в случае если для выравнивания не потребуется больше 12 пробелов. Если такое происходит необходимо либо отделить разбить свойства/переменные на группы
 
```coffeescript 
# -------- GOOD ---------

x           = getX()
y           = getY()
coordinates = [x, y]
  
extraLongVariableName = 3

# -------- BAD ----------

x                     = getX()
y                     = getY()
coordinates           = [x, y]
extraLongVariableName = 3
```
 
  * выравнивать `:` необходимо для каждого объекта по отдельности

```coffeescript 
# -------- GOOD ---------

options:
  patterns:
    "/^\[A-Z]/" : "const"
    "/^\_/"     : "private"

# -------- BAD ----------

options         :
  patterns      :
    "/^\[A-Z]/" : "const"
    "/^\_/"     : "private"
```

  * Если описание объекта содержит функции, то они должны быть описаны в конце и отделены пустыми строками (см. Пустые строки)
 
```coffeescript 
# -------- GOOD ---------

App.notify.fileupload
  url      : "#{url}/compile"
  multiple : true
  
  add: (e, data) =>
    file = data.files[0]
  
  yetAnotherFun: (e, data) =>
    # ...
    
# -------- BAD ----------

App.notify.fileupload
  url           : "#{url}/compile"
  multiple      : true
  add           : (e, data) =>
    file = data.files[0]
  yetAnotherFun : (e, data) =>
    # ...

```

  * если между свойствами присутствует комментарий или пустая строка, это считается "разбиением свойств на группы", и, в этом случае выравнивать свойства нужно отдельно для каждой группы

```coffeescript 
# -------- GOOD ---------
obj =
  x : 1
  y : 2
  
  # comment
  fooBar : 3
  option : 4
  
# -------- BAD ----------
obj =
  x      : 1
  y      : 2
  # comment
  fooBar : 3
  option : 4
```

#### Переменные

  * Для именования переменных необходимо использовать нотацию camelCase
  * Имя переменной должно нормально переводиться на русский язык и использовать правила построения предложений в английском языке
  * Имена приватных переменных, функций и методов должны начинаться с символа подчеркивания _

```coffeescript 
# -------- GOOD ---------

for key, value of options
  # ...
  
isConsistent = (originOption, currentOption) -> #... 
  
currentValue = null

# -------- BAD ----------

for k, v of opts
  # ...
 
cons = (a, b) -> # ...
 
cur = null
```

#### Функции
  * Для именования функций используйте правила именования переменных
  * Название функции должно обозначать действие (отвечать на вопрос "что сделать?")
  * Сохраняйте код функции небольшими. Если размер функции превышает 20 строчек кода (учитывая пустые строки), необходимо разделить функционал на 2 или более функции

```coffeescript 
# -------- GOOD ---------

getQuery = -> # ...

addCallbackToObserver = (observer, callback) -> # ...

preventFieldSerialization -> # ...

serializeList = -> # ...
# -------- BAD ----------

q = -> # ...
query = -> # ...

addToExcludeFieldFromSerialize -> # ...
 
SerializeList = -> # ...
serialize-list = -> # ...
serialize_list = -> # ...
```

  * Запрещается использование однострочной записи функций с использованием терминаторов

```coffeescript 
# -------- GOOD ---------

_getElement = (key) ->
  console.log key
  obj[key]

# -------- BAD ----------

_getElement = (key) -> console.log key; obj[key]
```

  * Не используйте директиву return в конце функции или метода

```coffeescript 
# -------- GOOD ---------

disableButton: ->
  @ui.button.attr "disabled", true

# -------- BAD ----------

disableButton: ->
  @ui.button.attr "disabled", true
  return
```

  * Однако, если в одной строке функция B используется при вызове функции A, то вызов функции B должен быть описан с использованием скобок.

```coffeescript 
# -------- GOOD ---------

preventFieldSerialization getFieldName(el)

classes = _ [@resolveNodeClass? node, model]
.flatten()
.compact()
.join " "

# -------- BAD ----------

preventFieldSerialization getFieldName el
preventFieldSerialization(getFieldName el)

classes = (_.compact _.flatten [@resolveNodeClass? node, model]).join " "
```

  * Запрещено определять более одной функции в одной строке

```coffeescript 
# -------- GOOD ---------

list
   .map (p) -> p.age
   .filter (a) -> a > 20

# -------- BAD ----------

list.map((p) -> p.age).filter((a) -> a > 20)
```

  * Запрещено описывать в одном выражении вызов нескольких функции, если результат выполнения одной является аргументом вызова другой функции, результат выполнения которой в свою очередь является аргументом для вызова третьей

```coffeescript 
# -------- GOOD ---------

date = _formatDate @getRunDate(model), DATE_FORMAT
el.select2 'val', date

# -------- BAD ----------

el.select2 'val', _formatDate(@getRunDate(model), DATE_FORMAT)
```

  * Используйте разрушение (destructuring) для раскрытия объекта в параметрах функции (TBD)

```coffeescript 
# -------- GOOD ---------

add = ({ a, b }, done) ->
   done null, a + b

# -------- BAD ----------

add = (params, done) ->
   done null, params.a + params.b
```

 * Толстую стрелку `=>` необходимо использовать только в случаях, когда внутри тела функции или метода необходимо переопределить указатель `this` на родительский объект
 * Для сохранения контекста используйте `=>` вместо описания переменных контекста (aka `_this`, `that`, `self`, etc)
 * Всегда используйте `@` вместо `this`

```coffeescript 
# -------- GOOD ---------

getQuery: ->
  _.each @attributes, (attr, key) =>
    if @computed[key] and @errors[key]
      # ...

# -------- BAD ----------

getQuery: ->
  _model = @
  computed = @computed
  _.each _model.attributes, (attr, key) ->
    if computed[key] and _model.errors[key]
      # ...
```

#### Строки
  * Для объединения строк используйте оператор интерполяции вместо простого объединения строк.
  * Всегда используйте двойные кавычки `""`

```coffeescript
# -------- GOOD ---------

@listenTo @model, "change:#{field} rollback", @update

# -------- BAD ----------

@listenTo @model, 'change:' + field + ' rollback', @update

```

#### Условия
  * _Условие_ должно быть описано _до блока_ инструкций, удовлеворяющих условию
  * Всегда используйте конструкцию `if`/`else` вместо конструкции  `unless`/`else`
  * Всегда используйте `if` для сложных условий

```coffeescript 
# -------- GOOD ---------

if not options.force and not mix.consistent
  # ...

if @loggedIn
  # ...
  
if not @loggedIn
  # ...
  
# -------- BAD ----------

unless options.force or mix.consistent
  # ...
  
return unless @loggedIn
return if not @loggedIn
```

#### Классы
  * Для именования классов используйте правила именования переменных, за исключением нотации: для именования классов используется натация PascalCase
  * Имя класса должно отражать сущность (отвечать на вопрос "кто?"/"что?") и дожно быть однозначным

```coffeescript 
# -------- GOOD ---------

class ArrayParser extends Parser


# -------- BAD ----------

class MixinArray extends Mixin
class App.Views.Analysis.UpdateEdit extends App.Views.Controls.DialogEdit 
```

#### Методы
  * Название метода должно обозначать действие (отвечать на вопрос "что сделать?"), исключения составляют названия методов, соответствующие соглашениям фремворка MarriontteJS
 
```coffeescript
# -------- GOOD ---------

getQuery: -> # ...

# -------- BAD ----------

query: -> # ...

# -------- OK -----------

onShow: -> # ...

```
 
  * Применяйте принципы единственной ответственности и сегрегации интерфейсов, если размер метода превышает 30 строчек кода (учитывая пустые строки), его необходимо декомпозировать
  * Имя метода обработчика действий пользователя должно отражать действие пользователя, а не поведение объекта класса

```coffeescript
# -------- GOOD ---------

triggers:
  "click @ui.remove": "remove:button:click"
  
onRemoveButtonClick: (e) ->
  # ...

# -------- BAD ----------

triggers:
  "click @ui.remove": "remove"
  
onRemove: (e) ->
  # ...
  
# -------- BAD ----------
  
triggers:
  "click @ui.remove": "remove:widget"
  
onRemoveWidget: (e) ->
  # ...

# -------- OK -----------

onShow: -> # ...

```

  * Методы, принимающие единственный аргумент "e", являются обработчиками событий Event и jQuery.Event, при этом:     - использование аргумента "e" в методах, не являющимися обработчиками таких событий, недопустимо
    - такие методы должны именоваться в соответствии с правилами именования приватных методов
   
```coffeescript 
# -------- GOOD ---------

class WidgetGridView

  events:
    "click @ui.remove": "_removeWidget"
    
  collectionEvents:
    "remove": "removeWidget"
    
  _removeWidget: (e) ->
    e.preventDefault()
    @removeWidget @collection.get(e.target.dataset.id)
    
  removeWidget: (widgetModel) ->
    # ...

# -------- BAD ----------

class WidgetGridView

  events:
    "click @ui.remove": "removeWidget"
    
  collectionEvents:
    "remove": "removeWidget"
    
  removeWidget: (e) ->
    widgetModel = if e.preventDefault
      @collection.get(e.target.dataset.id)
    else
      e
      
    # ...
  
```
  
#### Константы
  * Название констант должно быть написанно заглавными буквами, с элементом '_' в качестве разделителя

```coffeescript 
# -------- GOOD ---------

THIS_IS_CONSTANT = 3

# -------- BAD ----------

ThisIsConstant = 3
thisIsConstant = 3
THIS-IS-CONSTANT = 3
```

#### Объекты 
  * Не описывайте объекты с вложенными структурами в одну строчку

```coffeescript 
# -------- GOOD ---------

ui:
  replace: "[name=replace]"

# -------- BAD ----------

ui: replace: "[name=replace]"
```

#### Строковые  комментарии
  * Строковые комментарии необходимо располагать перед комментируемой строкой, но так чтобы комментарий был отделен пустой строкой от предыдущего логического блока
  * Комментрий пишется с большой буквы
  
```coffeescript 
# -------- GOOD ---------

getType = ->
  console.log 'проверяем тип...'
  
  # задаем тип по умолчанию 'no type'
  this._type or 'no type'

# -------- BAD ----------

getType = ->
  console.log 'проверяем тип...'
  type = this._type or 'no type' # задаем тип по умолчанию 'no type'
  return type

```

  * При комменрировании значений списков и объектов комментарии следует располагать справа от значений, при условии что комментарий умещается в строке

```coffeescript 
# -------- GOOD ---------

types: [
  "object_type_code"   # Тип события
  "category"           # Категории
  "protected_document" # Объекты защиты
]

# -------- BAD ----------

types: [
  # Тип события
  "object_type_code"
  # Категории  
  "category"        
  # Объекты защиты  
  "protected_document"
]
```

#### Блочные комментарии
  * Блочный комментарий задается с использованием стандарта ###*
  * Комментрий пишется с большой буквы
  * Для отделения логических блоков внутри комментария используется разделитель в одну пустую строку
  * Блочный комментарий должен быть отделен от вышеописанного кода пустой строкой

```coffeescript 
# -------- GOOD ---------

@init()
 
###*
 * This is a block comment. Note that if this were a real block
 * comment, we would actually be describing the proceeding code.
 *
 * This is the second paragraph of the same block comment. Note
 * that this paragraph was separated from the previous paragraph
 * by a line containing a single comment character.
###
@start()
@stop()

# -------- BAD ----------

@init()
###
 this is a block comment. Note that if this were a real block
 comment, we would actually be describing the proceeding code.
 This is the second paragraph of the same block comment. Note
 that this paragraph was separated from the previous paragraph
 by a line containing a single comment character.
###
@start()
@stop()

```

#### Документирование классов
  * Для документирования классов и методов используются аннотации Codo с использованием фигурных скобок 
  * При наличии виртуальных/абстрактных методов необходимо использовать @method
  
```coffeescript 
# -------- GOOD ---------

###*
 * Class to service fancytree with data
 *
 * @method #prepareNode(node, model)
 *   Prepare node to be involved to fancytree
 *   @param {Object} node
 *   @param {Backbone.Model} model
 *
 * @method #resolveNodeClass(node, model)
 *   Add additional classes to fancytree node
 *   @param {Object} node
 *   @param {Backbone.Model} model
 *
###
module.exports = class TreeCollection extends Backbone.Collection

    rootId: null

```

  * Группируйте методы и функции с помощью соответвующих комментариев, как описано ниже:
    - `# PRIVATE` - приватные методы, которые не дожны вызываться извне
    - `# PUBLIC` - публичные методы, формирующие интерфейс класса. Необходимо помнить что если метод не приватный - еще не значит что он является интерфейсом класса, это может быть обработчик события и в соответствии с соглашениями фреймворка иметь название, не начинающееся с `_`
    - `# ENCLOSED/PROTECTED (TBD)` - функции, скрытые замыканием 
    
  * Описывать свойства и методы нужно в следующем порядке:
    - Свойства и методы класса
    - Свойства прототипа, в том числе вычисляемые через _.result (`idAttribute`, `url`, `defaults`, `events`, `triggers`, `behaviors`, etc...)
    - Конструктор и `initialize`
    - методы Backbone-а, Marionett-а (`onShow`, `beforeDestroy`, обработчики триггеров и все методы с тегом `@implements`
    - PROTECTED
    - PRIVATE
    - PUBLIC
  
*При таком порядке описания свойств и методов в начале описания класса будут наиболее ожидаемые свойства и методы, затем "внутреняя кухня" и интерфейс* 

```coffeescript 
# -------- GOOD ---------

            ###*
             * ...
            ###
            defaultOptions: []
            
            ###*
             * ...
            ###
            initialize: ->
              # ...

# 1 ......................................................................... 80
#                                                                              ↓
            ###################################################################
            # PROTECTED

            ###*
             * Route handlers presenter involves common dom and state
             * manipulations for each handler e.g. sidebar tree view rendering
             * @param  {String} type - route main entity type (report/folder)
             * @param  {Function} handler - route handler
            ###
            _presenter = (type, handler) ->
                (id, tail...) ->

            # ...

            ###################################################################
            # PRIVATE

            ###*
             * Get query from collection
             * @param  {Number} id
             * @return {Backbone.Model}
            ###
            _getQuery: (id) ->
              query = @queries.get id
                
            # ... 
            
            ###################################################################
            # PUBLIC

            ###*
             * Show empty view
            ###
            show: _presenter "empty", ->
              @tree.resetNodesActivity()
              @regions.content.show new EmptyView

```

#### Документирование методов
  * Документирование методов является обязательным
  * Сначала следует описание логики метода, при этом описание должно быть информативным и не должно повторять название метода
  * Если метод должен возвращать значение, то присутствие тега @return обязательно, иначе тега `@return` быть не должно
  
```coffeescript 
# -------- GOOD ---------

###*
 * Wrap function with try/catch, handle error and
 * push it to the server with stacktrace
 * @param {Function} fn - function to be wrapped
 * @return {Function} wrapped function
###
wrapMethod = (fn) ->
  (args...) ->
    # ...

# -------- BAD ----------

###*
 * Create a wrapper function
###
wrapMethod = (fn) ->
  (args...) ->
    # ...

```

* В случае, если метод абстрактный, должен быть тег @implements с указанием абстрактного класса

```coffeescript 
# -------- GOOD ---------

class ReportsTree extends FancyTree

  # ...
  
  ###*
   * Show report on reports tree item click
   * @implements FancyTree
   * @param {Object} node - fancytree node
   *
  ###
  onItemClick: (node) ->
  

# -------- BAD ----------

winner = yes if pick in [47, 92, 13]

```

  * В случае, если объявление метода переопределяет метод супер-класса, методу необходимо добавить тег @override
 
```coffeescript
class HistoryGrid extends Grid

  ###*
   * Register event handlers
   * @override
  ###
  onShow: ->
    super
    # ...
```

#### Аннотирование кода
  * Используйте аннотации, когда необходимо описать конкретные действия, которые должны быть приняты в отношении указанного блока кода
  * Аннотация должна располагаться непосредственно над кодом, который аннотация описывает. 
  * Ключевое слово аннотации должно заканчиваться двоеточием и пробелом

  * В коде могут использоваться следующие виды аннотаций:
    -  `TODO: ` - для указания проблемы, к которой нужно вернуться в дальнейшем или если вы предлагаете решение проблемы, которое должно быть реализовано
    - `FIXME: ` - указывает на неработоспособный код, который должен быть исправлен
    - `OPTIMIZE: ` - описывают код, который является неэффективным и может стать узким местом
    - `HACK: ` - указывает на сомнительное решение задачи

#### 

```coffeescript 
# FIXME: The client's current state should *not* affect payload processing.
resetClientState()
processPayload()
  
# TODO: Ensure that the value returned by this call falls within a certain
# range, or throw an exception.
analyze()
```

#### Псевдонимы
  * Всегда используйте псевдонимы операторов - `is`, `isnt`, `not`, `and`, `or`, `or=`

```coffeescript 
# -------- GOOD ---------

if param is 'test' and param isnt 'foo' then
  # code here
 
if param isnt 'foo' or param isnt 'bar' then
  # code here

temp or= {}

# -------- BAD ----------

if param == 'test' and param != 'foo' then
   # code here
 
if param != 'foo' or param != 'bar' then
   # code here
 
temp = temp || {}
```

  * Не используйте булевые псевдонимы - on, off, yes, no

```coffeescript 
# -------- GOOD ---------

winner = true if pick in [47, 92, 13]

# -------- BAD ----------

winner = yes if pick in [47, 92, 13]
```

#### Генераторы списков
  * Старайтесь использовать map/reduce функции или генераторы вместо стандартных вариаций наполнения списка

```coffeescript 
# -------- GOOD ---------

results = _.filter array, (item) -> item.title is "test

surnames = _ users
.filter name: "John"
.pluck 'surname'

# -------- BAD ----------

results = []
for item in array
  if item.name is "test"
    results.push item.name
    
# -------- OK ----------
    
surnames = (user.surname for user in users when user.name is "John")
```

#### JQuery
  * Для jQuery-переменных используйте префикс `$` (TBD)
  * Кэшируйте jQuery-запросы. Каждый новый jQuery-запрос делает повторный поиск по DOM-дереву, и приложение начинает работать медленнее

```coffeescript 
# -------- GOOD ---------

setSidebar = ->
  $sidebar = $('.sidebar')
  $sidebar.hide()

  # ...

  $sidebar.css
    'background-color': 'pink'

# -------- BAD ----------

setSidebar = ->
  $('.sidebar').hide()

  # ...

  $('.sidebar').css
    'background-color': 'pink'

```

  * Не создавайте большое количество обработчиков. Создавайте обработчики для родительских DOM-элеметов с использованием `event.target`
  * Используйте *только* атрибуты `data` для поиска элементов и извлечения данных из них
  
```coffeescript 
# -------- GOOD ---------

$.sidebar = $(".sidebar")
$.sidebar.on "click", (e) =>
  if id = e.target.dataset["user-id"]
    @trigger "show:user", id

# -------- BAD ----------

$.users = $(".sidebar > li")
$.users.on "click", (e) =>
  if id = e.currentTarget.dataset["user-id"]
    @trigger "show:user", id

```

#### Импортирование модулей
  * При импортировании модулей каждая директива  REQUIRE должна располагаться на отдельной строчке
  * При импортирование модулей необходимо придерживаться следующим правилам группировки директив импорта:
    - импортирование модулей внешних библиотек
    - импортирование общих модулей продукта
    - импортирование модулей, локальных для данного раздела
  

```coffeescript 
# -------- GOOD ---------

require 'lib/setup'
Backbone = require 'backbone'
Selection = require "models/events/selection.coffee"

```

  * Если импортированный модуль представляет из себя объект, следует сохранять ссылку на весь объект
  
```coffeescript 
# -------- GOOD ---------

helpers = require "path/to/helpers.coffee"

# ...
helpers.formatDate date

# -------- BAD ----------

formatDate = (require "path/to/helpers.coffee").formatDate

# ...
formatDate date
```

#### Обработка событиий
  * При описании обработчиков в свойстве events:
    - Имена методов должны начинаться с подчеркивания `_`
    - Имена методов должны отображать действие, которое выполнится при возникновениисобытия (`_removeUser`, а не `_onClickRemove`)
    - Все описания событий должны использовать `@ui`, если это поддержавается классом (в регионах, к примеру, нет поддержки ui)

```coffeescript 
# -------- GOOD ---------

ui:
  usersList : "[data-users]"
  users     : "[data-user]"

events: 
  "click @usersList": "_checkUser"
  
_checkUser: (e) ->
  e.preventDefault()
  if el = $(e.target).find "[data-user]"
    @users.get el.data "id"
    .set "selected", true
  

# -------- BAD ----------

ui:
  usersList : "ul.usersList"
  users     : "ul.usersList > li"

events: 
  "click ul.usersList > li": "onUserClick"
  
onUserClick: (e) ->
  e.preventDefault()
  if el = $(e.target).find "[data-user]"
    @users.get el.data "id"
    .set "selected", true

```


#### Именование файлов исходного кода

```coffeescript 
# -------- GOOD ---------



# -------- BAD ----------


```

#### Использование data-атрибутов
 * Имена атрибутов должны содержать сооветствовать выражению `/^[a-z\-]+$/`
 * Значения должны иметь только недетерминированные дата-атрибуты (однородные с идеологической точки зрения)

```coffeescript 
# -------- GOOD ---------

ui:
  item      : "[data-collapse-item]"
  container : "[data-collapse-container]"
  
  # actions
  save   : "[data-action='save']"
  delete : "[data-action='delete']"

# -------- BAD ----------

ui:
  item      : "[data-collapse=item]"
  container : "[data-collapse=container]"
  
  # actions
  save    : "[data-action-save]"
  delete  : "[data-action-delete]"

```

#### 

```coffeescript 
# -------- GOOD ---------



# -------- BAD ----------


```

#### Консистенсистентность данных модели
Backbone модель - отражает сущность в системе, содержающую набор атрибутов. Значениея атрибутов могут быть представлены в виде:
 - скалярной величины
 - объекта или массива
, при этом в случае если значение атрибута модели является структурой, то оно может содержать в себе, как данные, неразрывно связанные с атрибутами модели, так и данные другой сущности. Обусловлено это тем, что бекенд умеет отдавать связанные сущности в качестве атрибутов.

Задачи, решаемые алгоритмистами и программистами, в большей части связаны с поиском эффективной структуры данных и реализацией механизмов поддержки её консистентности. Консистентность структуры данных в теории алгоритмов имеет важное значение. Исходя из этого, наиболее эффективной структурой будет являться та, поддержание консистентности которой будет наименее трудозатратной и простой. Объективно, поддерживать JSON структуру, лишенную таких данных как методы, цепь прототипов и циклические ссылки, несомненно проще.

Пример данных модели "Запрос", приходящих с бекенда:

```javascript 
# -------- GOOD ---------
 {  
   "QUERY_ID":1667,
   "HASH":"836ED2224E5896CA2AC1A60BB0CD98E58534D570",
   "QUERY_TYPE":"query",
   "QUERY":"{\"mode\":\"lite\",\"data\":{\"link_operator\":\"and\",\"children\":['more data...']}}",
   "DISPLAY_NAME":"65541",
   "USER_ID":22,
   /* more data */
   "user":{  
     "USER_ID":22,
     "USER_TYPE":4,
     "PROVIDER":"PASSWORD",
     /* more data */
   }
 }
```

Модель "Запрос" содержит 2 вложенные структуры: QUERY и user. При этом данные структуры QUERY неразрывно связаны с моделью "Запрос" и не могу существовать независимо от нее. Данные структуры user придназначены для модели "Пользователь", а значит модель "Запрос" не должна содержать эти данные, но может использоваться для их получения с сервера. Вариант размещения в атрибутах модели "Пользователь" возможен, но как и любая неконсистентная структура данных накладывает ограничения и создает сложности для дальнейшей её поддержки. Примером ограничения может служить невозможность (без лишних манипуляций) сериализации в JSON только данных модели "Запрос". Сложность поддержки может обуславливаться появлением подолнительных типов данных в структуре объекта, нередко содержащих циклические ссылки (например в случае с Backbone коллекцией внутри аттрибутов). 

В связи с вышесказанным, данные, не принадлежащие модели должны быть обработаны и исключены из набора аттрибутов модели при получении данных с сервера.

Пример обработки:

```coffeescript 
# -------- GOOD ---------

  parse: (data) =>
    data = super

    if user = data.user
      @user ?= new UserModel
      @user.set data.user
      
    _.omit data, 'user'

# -------- BAD ----------


```
