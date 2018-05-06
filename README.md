# sqlcoin

### Architecture

Public utility distributed database to record ownerships and assets online.

### Ownership

* Single user
* OR for two users (good for family)
* AND for two users (good for family)
* OR for list of users (good for list of users with equal rights, for example LLP)
* List of users with shares (%) (good for LLC, S-Corp)
* List of users with stocks (quantity) (good for C-Corp)
* Proxy

### PII

PII data must not be stored in public utility database.

### Verification

Users can be verified by another users and came to special lists.

### Collectibles

* Collectible elements must not be negative.
* Collectible elements can not be seized.
* Collectible elements can be debited to anybody.
* Collectible elements can be credited only by signature(signatures) of owner(s).
* Collectible elements must have limited quantity (100%, 100000 shares, 10000000 coins, 1 identified item).

### Identifier Names

* ASCII: [0-9,a-z,A-Z$_]
* Extended: U+0080 .. U+FFFF
* Identifiers are stored as Unicode (UTF-8)
* Identifiers are case-insensitive
* Identifiers can't end with space characters
* Identifiers are not permitted to contain the ASCII NUL character (U+0000) and supplementary characters (U+10000 and higher)
* Identifier names may begin with a numeral, but can't only contain numerals
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
   asset varchar(128),
   owner user,
   primary key (asset),
   constraint asset_col collectible (asset) signed by (owner)
);

update asset1 set owner='bob' where asset='picture_num_123'; -- signed by 'alice' and previous owner is 'alice', 'bob' no need to sign this

update asset1 set owner='bob' where asset in ('picture_num_123', 'realestate_num_333'); -- signed both by 'alice' and 'craig' as prevoius owners of assets, no need signature of bob


create table asset2 (
   asset varchar(128),
   owner1 user,
   owner2 user,
   primary key (asset),
   constraint asset_col collectible (asset) signed by any(owner1, owner2)
);

create table asset3 (
   asset varchar(128),
   owner1 user,
   owner2 user,
   primary key (asset),
   constraint asset_col collectible (asset) signed by all(owner1, owner2)
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
