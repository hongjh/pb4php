# Protocol Buffer for PHP (缩写为pb4php)

protocolbuf_025.zip 包

> protocolbuf 官方已支持php版本，下载地址：

> https://github.com/google/protobuf/releases , 选 protobuf-php-3.6.0.zip

> 支持 php 扩展安装 或 composer

## 相关类库
- https://github.com/allegro/php-protobuf
- https://github.com/drslump/Protobuf-PHP
- http://code.google.com/p/pb4php/downloads/list

## 下载地址

https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/pb4php/protocolbuf_025.zip

## 使用说明

I implemented a protocol buffer version for php with native parseing of .proto files and php native encoding and decoding of messages. Addressbook represented through a .proto file

```
message Person
{
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    required string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }
  // a simple comment
  repeated PhoneNumber phone = 4;
  optional string surname = 5;
}

message AddressBook {
  repeated Person person = 1;
}
```

and here the php code to create a person and save the complete addressbook to test.pb

```
$book = new AddressBook();
$person = $book->add_person();
$person->set_name('Kordulla');
$person->set_surname('Nikolai');

$phone_number = $person->add_phone();
$phone_number->set_number('49');
$phone_number->set_type(Person_PhoneType::WORK);

$phone_number = $person->add_phone();
$phone_number->set_number('171');
$phone_number->set_type(Person_PhoneType::MOBILE);

// serialize
$string = $book->SerializeToString();

// write it to disk
file_put_contents('adressbook.pb', $string);
```

 

To read the addressbook and the saved person just do this:

```
$string = file_get_contents('./test.pb');

// Just read it
$book = new AddressBook();
$book->parseFromString($string);

var_dump($book->person_size());
$person = $book->person(0);
var_dump($person->name());
var_dump($person->surname());
var_dump($person->phone(0)->number());
var_dump($person->phone(0)->type());
var_dump($person->phone(1)->number());
var_dump($person->phone(1)->type());
```

 

For read further example and code snippet just go to http://code.google.com/p/pb4php/ . You can download the sources with example.