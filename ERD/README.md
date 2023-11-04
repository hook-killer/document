# ERD

- [DBDocs Link](https://dbdocs.io/donsonioc2010/Hook_killer)

## 개발서버 배포시 적용해야 하는 DDL

```SQL
create table certificate_key
(
    expriation_at datetime(6)  null,
    id            bigint auto_increment
        primary key,
    uuid_key      varchar(255) not null
);

create table tbl_board
(
    board_id    bigint auto_increment
        primary key,
    board_type  enum ('CHINA', 'JAPAN', 'KOREA') null,
    description varchar(255)                     not null,
    name        varchar(255)                     not null
);

create table tbl_refresh_token
(
    id            bigint       not null
        primary key,
    ttl           bigint       null,
    refresh_token varchar(255) null
);

create table tbl_user
(
    created_at timestamp default CURRENT_TIMESTAMP                   not null,
    id         bigint auto_increment
        primary key,
    update_at  timestamp default CURRENT_TIMESTAMP                   not null,
    email      varchar(255)                                          null,
    login_type enum ('DEFAULT', 'GOOGLE', 'KAKAO', 'LINE')           null,
    nick_name  varchar(255)                                          null,
    oid        varchar(255)                                          null,
    password   varchar(255)                                          null,
    provider   enum ('KAKAO')                                        null,
    role       enum ('ADMIN', 'USER')                                null,
    status     enum ('ACTIVE', 'DELETE', 'NOT_ACTIVE', 'SUSPENSION') null,
    thumbnail  varchar(255)                                          null,
    constraint UK_npn1wf1yu1g5rjohbek375pp1
        unique (email)
);

create table tbl_article
(
    like_count           int                                 not null,
    org_article_language tinyint                             not null,
    board_id             bigint                              null,
    created_at           timestamp default CURRENT_TIMESTAMP not null,
    created_user_id      bigint                              not null,
    id                   bigint auto_increment
        primary key,
    update_at            timestamp default CURRENT_TIMESTAMP not null,
    updated_user_id      bigint                              not null,
    article_status       enum ('DELETE', 'PUBLIC')           not null,
    constraint FK7mji0p04yej7n7ur01cfo7kc2
        foreign key (created_user_id) references tbl_user (id),
    constraint FKs9li3q4spsnuc8d6sct7qg6pu
        foreign key (updated_user_id) references tbl_user (id),
    constraint FKt5972awg5uh3h2r8la7fi9gmq
        foreign key (board_id) references tbl_board (board_id),
    check (`org_article_language` between 0 and 3)
);

create table tbl_article_content
(
    article_id bigint                        not null,
    id         bigint auto_increment
        primary key,
    user_id    bigint                        null,
    language   enum ('CN', 'EN', 'JP', 'KO') null,
    title      varchar(255)                  not null,
    content    tinytext                      not null,
    constraint FKdikbc8d20uo6j0vl4u24xp6bk
        foreign key (article_id) references tbl_article (id)
);

create table tbl_article_like
(
    article_id bigint                              null,
    created_at timestamp default CURRENT_TIMESTAMP not null,
    id         bigint auto_increment
        primary key,
    update_at  timestamp default CURRENT_TIMESTAMP not null,
    user_id    bigint                              null,
    constraint FK7g66vsrcx77ywfiwb5sglsqcf
        foreign key (user_id) references tbl_user (id),
    constraint FKkqh7abg9d9eep6c3brin5pksc
        foreign key (article_id) references tbl_article (id)
);

create table tbl_notice_article
(
    created_at      timestamp default CURRENT_TIMESTAMP not null,
    created_user_id bigint                              null,
    id              bigint auto_increment
        primary key,
    update_at       timestamp default CURRENT_TIMESTAMP not null,
    updated_user_id bigint                              null,
    language        enum ('CN', 'EN', 'JP', 'KO')       null,
    status          enum ('DELETE', 'PUBLIC')           null,
    constraint FKdob4i41tcy0kmxm099inbqbxn
        foreign key (created_user_id) references tbl_user (id),
    constraint FKky60j0m379wpjk43c6n6s2vfi
        foreign key (updated_user_id) references tbl_user (id)
);

create table tbl_notice_content
(
    id        bigint auto_increment
        primary key,
    notice_id bigint                        null,
    language  enum ('CN', 'EN', 'JP', 'KO') null,
    title     varchar(255)                  null,
    content   tinytext                      null,
    constraint FKlr5dadvqukjojqmopxo6ignso
        foreign key (notice_id) references tbl_notice_article (id)
);

create table tbl_reply
(
    org_reply_language tinyint                             not null,
    article_id         bigint                              null,
    created_at         timestamp default CURRENT_TIMESTAMP not null,
    created_user_id    bigint                              not null,
    id                 bigint auto_increment
        primary key,
    update_at          timestamp default CURRENT_TIMESTAMP not null,
    updated_user_id    bigint                              not null,
    reply_status       enum ('FALSE', 'TURE')              not null,
    constraint FK5swc2hw1rgnpgea6ioru5jvtt
        foreign key (created_user_id) references tbl_user (id),
    constraint FKd7oatexnaahs4f4u7vpp53u57
        foreign key (article_id) references tbl_article (id),
    constraint FKdkg7r4uixwcrdxtglxrcvn4ao
        foreign key (updated_user_id) references tbl_user (id),
    check (`org_reply_language` between 0 and 3)
);

create table tbl_reply_content
(
    language tinyint  not null,
    id       bigint auto_increment
        primary key,
    reply_id bigint   null,
    content  tinytext null,
    constraint FKgn1tdk3egfqakip0tmra2amj5
        foreign key (reply_id) references tbl_reply (id),
    check (`language` between 0 and 3)
);

create table tbl_resources
(
    created_at   timestamp default CURRENT_TIMESTAMP           not null,
    created_user bigint                                        not null,
    id           bigint auto_increment
        primary key,
    update_at    timestamp default CURRENT_TIMESTAMP           not null,
    path         varchar(255)                                  not null,
    usage_type   enum ('ADMIN', 'ARTICLE', 'BOARD', 'PROFILE') not null,
    constraint FKs0dvf5c2gcfmmet0tvrw5a4f9
        foreign key (created_user) references tbl_user (id)
);




insert into tbl_board (name, board_type, description) values("한국", "KOREA", "한국어게시판");
insert into tbl_board (name, board_type, description) values("일본", "JAPAN", "일본어게시판");
insert into tbl_board (name, board_type, description) values("중국", "CHINA", "중국어게시판");
```
