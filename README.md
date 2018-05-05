# sqlcoin

### System table

```

create table users (
   nickname varchar(64),
   type char(1),   // 'I' - individual, 'C' - corporation, 'P' - partnership, 'T' - trust
   algo varchar(20),
   pubkey blob(65),
   restore varchar(64),
   stakeholders table (
      nickname varchar(64),
      constraint nick_ref
      foreign key (nickname)
      references users(nickname),
   ),
   constraint update signed by (nickname, restore, quorum(stackholders))
);

```

### Tokens

```
create table sqlcoin (
   owner varchar(64),
   amount decimal(10, 6),
   constraint owner_ref
    foreign key (nickname)
    references users(nickname),
   constraint amount_chain collectible(amount)
);

money transfer transaction:

update table sqlcoin debit amount on @1 where owner=‘bob’;
update table sqlcoin credit amount on @1 where owner=‘alice’;
signed by ‘alice’;
```

### Assets

```
create table 

```
