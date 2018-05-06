# sqlcoin

### Identifier Names

* ASCII: [0-9,a-z,A-Z$_]
* Extended: U+0080 .. U+FFFF
* Identifiers are stored as Unicode (UTF-8)
* Identifiers are not case-sensitive
* Identifiers can't end with space characters
* Identifiers are not permitted to contain the ASCII NUL character (U+0000) and supplementary characters (U+10000 and higher)
* Identifier names may begin with a numeral, but can't only contain numerals unless quoted
* Identifier maximum length is 64 characters
* Identifier can not be a reserve word

### Namespace

* Nickname of the user is the identifier
* Nickname is using as database name

### System tables

Nickname 'system' is reserved. It defines a system database.
All users in the system are placed in a one table 'users'.

Definition of the table 'users':
```
create table system.users (
   nickname varchar(64) not null,
   algo varchar(10) not null, // 'ECDSA'
   pubkey varblob(256) not null,
   restorekey varblob(256) default null,
   primary key (nickname),
   constraint signed by (select pubkey, restorekey)
);

```

### White List

```
create table us_white_list (
  nickname varchar(64),
  primary key (nickname),
  constraint nick_ref foreign key (nickname) references users(nickname)
);

create table us_only_coin (
   owner varchar(64),
   amount decimal(10, 6),
   primary key (owner),
   constraint owner_ref foreign key (owner) references us_white_list(nickname),
   constraint sum(amount) == 100000000,
   constraint amount >= 0.0,
   constraint collectible amount
);

```

### Tokens

```
create table sqlcoin (
   owner varchar(64),
   amount decimal(10, 6),
   primary key (owner),
   constraint owner_ref foreign key (owner) references users(nickname),
   constraint sum(amount) == 100000000,
   constraint amount >= 0.0,
   constraint collectible amount
);

money transfer transaction:

update table sqlcoin debit(amount, @1) where owner=‘bob’;
update table sqlcoin credit(amount, @1) where owner=‘alice’;
signed by ‘alice’;
```

### Assets

```
create table asset1 (
   owner varchar(64),
   asset varchar(64),
   primary key (asset),
   constraint owner_ref foreign key (owner) references users(nickname),
   constraint asset_unique unique(asset),
   constraint collectible asset
);

```

### Organizations

```
create table org1 (
   stakeholder varchar(64),
   share decimal(3,4),
   primary key (stakeholder),
   constraint stakeholder_ref foreign key (stakeholder) references users(nickname),   
   constraint sum(share) == 100.0,
   constraint amount > 0,
   constraint collectible amount,
);
```
