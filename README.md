# sqlcoin

### System table

```

create table users (
   nickname varchar(64),
   algo varchar(20), // 'ECDSA'
   pubkey blob(65),
   restorekey blob(65),
   primary key (nickname),
   owner by nickname
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

update table sqlcoin debit amount on @1 where owner=‘bob’;
update table sqlcoin credit amount on @1 where owner=‘alice’;
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
