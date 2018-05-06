# sqlcoin

### System table

```

create table users (
   nickname varchar(64),
   type char(1),   // 'I' - individual, 'C' - corporation, 'P' - partnership, 'T' - trust
   algo varchar(20), // 'ECDSA'
   pubkey blob(65),
   restore varchar(64),
   stakeholders table (
      nickname varchar(64),
      constraint nick_ref foreign key (nickname) references users(nickname)
   ),
   constraint restore_ref foreign key (nickname) references users(nickname),
   constraint update signed by any(nickname, restore, quorum(stackholders))
);

```

### Tokens

```
create table sqlcoin (
   owner varchar(64),
   amount decimal(10, 6),
   constraint owner_ref foreign key (owner) references users(nickname),
   constraint sum(amount) == 100000000,
   constraint amount >= 0.0,
   constraint collectible amount
) issued by 'alice';

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
   constraint owner_ref foreign key (owner) references users(nickname),
   constraint asset_unique unique(asset)
   constraint collectible asset
);

```

### Organizations

```
create table org1 (
   stakeholder varchar(64),
   share decimal(3,4),
   constraint stakeholder_ref foreign key (stakeholder) references users(nickname),   
   constraint sum(share) == 100.0,
   constraint amount > 0,
   constraint collectible amount,
) issued by 'alice';
```
