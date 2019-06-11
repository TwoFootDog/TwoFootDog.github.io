---
layout: post
title: "[MariaDB]MariaDB 테이블 명령어"
description: 
image: '../images/강아지66.jpg'
category: 'MariaDB'
tags : 
- IT
- MariaDB

twitter_text: 
introduction : MariaDB 테이블 명령어
---

### [1. 개요]
MariaDB 테이블 명령어를 알아보자

_ _ _



### [2. 테이블 명령어]
```


CREATE TABLE APR_BATSCH_MST (
  MBRSH_PGM_ID  VARCHAR(1) NOT NULL,
  MST_BAT_ID    VARCHAR(20) NOT NULL,
  MST_BAT_NM    VARCHAR(50),
  HOST_NM       VARCHAR(20),
  INPUT_FILE_YN VARCHAR(1),
  FILE_ID       VARCHAR(10),
  HNM_FILE_NM   VARCHAR(40),
  PRE_BAT_EXIST_YN  VARCHAR(1),
  FILE_AUTO_SEND_YN VARCHAR(1),
  MULTI_PROC_CNT    BIGINT,
  REPORT_TYP        VARCHAR(1),
  APL_YN            VARCHAR(1),
  REGR_ID           VARCHAR(8),
  REG_DT            DATETIME,
  UPDR_ID           VARCHAR(8),
  UPD_DT            DATETIME
)ENGINE=INNODB;

ALTER TABLE APR_BATSCH_MST ADD CONSTRAINT PRIMARY KEY (MBRSH_PGM_ID, MST_BAT_ID);

CREATE TABLE APR_BATPRO_LST (
  MBRSH_PGM_ID      VARCHAR(1) NOT NULL,
  MST_BAT_ID        VARCHAR(20) NOT NULL,
  WORK_SEQ          BIGINT NOT NULL,
  MULTI_PROC_SEQ    BIGINT NOT NULL,
  PGM_ID            VARCHAR(20),
  PGM_CD            VARCHAR(6),
  BAT_EXE_FILE_PATH_NM  VARCHAR(300),
  LOG_FILE_DESC         VARCHAR(2),
  LOG_fILE_PATH_NM      VARCHAR(300),
  BAT_PARAM_DM          VARCHAR(1),
  BAT_PARAM_CNT         BIGINT,
  BAT_PARAM_VAL_CNTS1   VARCHAR(100),
  BAT_PARAM_VAL_CNTS2   VARCHAR(100),
  BAT_PARAM_VAL_CNTS3   VARCHAR(100),
  BAT_PARAM_VAL_CNTS4   VARCHAR(100),
  BAT_PARAM_VAL_CNTS5   VARCHAR(100),
  BAT_PARAM_VAL_CNTS6   VARCHAR(100),
  BAT_PARAM_VAL_CNTS7   VARCHAR(100),
  BAT_PARAM_VAL_CNTS8   VARCHAR(100),
  BAT_PARAM_VAL_CNTS9   VARCHAR(100),
  BAT_PARAM_VAL_CNTS10   VARCHAR(100),
  REGR_ID               VARCHAR(8),
  REG_DT                DATETIME,
  UPDR_ID               VARCHAR(8),
  UPD_DT                DATETIME
)ENGINE=INNODB;

ALTER TABLE APR_BATPRO_LST ADD CONSTRAINT PRIMARY KEY (MBRSH_PGM_ID, MST_BAT_ID, WORK_SEQ, MULTI_PROC_SEQ);


CREATE TABLE APR_PRFBAT_MST (
  MBRSH_PGM_ID VARCHAR(1) NOT NULL,
  MST_BAT_ID   VARCHAR(20) NOT NULL,
  PRE_MST_BAT_ID  VARCHAR(20) NOT NULL,
  APL_YN          VARCHAR(1),
  REGR_ID         VARCHAR(8),
  REG_DT          DATETIME,
  UPDR_ID         VARCHAR(8),
  UPD_DT          DATETIME
)ENGINE=INNODB;

ALTER TABLE APR_PRFBAT_MST ADD CONSTRAINT PRIMARY KEY (MBRSH_PGM_ID, MST_BAT_ID, PRE_MST_BAT_ID);


CREATE TABLE COM_BATRES_TRN (
  MBRSH_PGM_ID VARCHAR(1) NOT NULL,
  FILE_ID       VARCHAR(10) NOT NULL,
  FILE_NM       VARCHAR(40) NOT NULL,
  WORK_DY       VARCHAR(8),
  WORK_FR_TM    VARCHAR(6),
  WORK_STS      VARCHAR(2),
  WORK_TO_TM    VARCHAR(6),
  RCV_DY        VARCHAR(8),
  RCV_TM        VARCHAR(6),
  TGT_CNT       BIGINT,
  ERR_CNT       BIGINT,
  RES_FILE_NM1  VARCHAR(100),
  RES_FILE_NM2  VARCHAR(100),
  BRD_REQ_NO    VARCHAR(10),
  REGR_ID       VARCHAR(8),
  REG_DT        DATETIME,
  UPDR_ID       VARCHAR(8),
  UPD_DT        DATETIME,
  RES_FILE_NM3  VARCHAR(100),
  RES_FILE_NM4  VARCHAR(100)
)ENGINE=INNODB;

ALTER TABLE COM_BATRES_TRN ADD CONSTRAINT PRIMARY KEY (MBRSH_PGM_ID, FILE_ID, FILE_NM);


CREATE TABLE COM_BATFILE_TRN (
  MBRSH_PGM_ID    VARCHAR(1) NOT NULL,
  FILE_ID         VARCHAR(10) NOT NULL,
  FILE_NM         VARCHAR(40),
  TRANS_RCV_FG    VARCHAR(1),
  TRANS_ORGAN_CD  VARCHAR(4),
  RCV_ORGAN_CD    VARCHAR(4),
  STS_CD          VARCHAR(1),
  WORK_STS        VARCHAR(1),
  SEND_FR_DT      DATETIME,
  SEND_TO_DT      DATETIME,
  FILE_SZ         BIGINT,
  TOT_REC_CNT     BIGINT,
  SEND_REC_CNT    BIGINT,
  MSG             VARCHAR(100),
  REGR_ID         VARCHAR(8),
  REG_DT          DATETIME,
  UPDR_ID         VARCHAR(8),
  UPD_DT          DATETIME,
  FILE_NAME       VARCHAR(40),
  RCV_DY          VARCHAR(8)
)ENGINE=INNODB;





CREATE SEQUENCE SYS_ID_SEQ START WITH  1 INCREMENT BY 1 MAXVALUE 99999;

CREATE TABLE SYS_INF_MST
(SYS_ID VARCHAR(8) NOT NULL,
SYS_NM VARCHAR(50),
AVL_TO_DY DATETIME NOT NULL,
AVL_FR_DY DATETIME NOT NULL,
APL_YN VARCHAR(1) NOT NULL )
ENGINE=INNODB;

ALTER TABLE SYS_INF_MST ADD CONSTRAINT SYS_INF_MST_PK PRIMARY KEY (SYS_ID, AVL_TO_DY);

INSERT INTO SYS_INF_MST VALUES (CONCAT('SYS', LPAD(NEXTVAL(SYS_ID_SEQ), 5, 0)),
                                'Nxmile Batch Visualizer',
                                SYSDATE(),
                                SYSDATE(),
                                'Y');

CREATE SEQUENCE CONN_ID_SEQ START WITH 1 INCREMENT BY 1 MAXVALUE 999999;

CREATE TABLE SYS_CON_MST
(CONN_ID VARCHAR(10) NOT NULL,
AVL_TO_DY DATETIME NOT NULL,
AVL_FR_DY DATETIME  NOT NULL,
APL_YN VARCHAR(1) NOT NULL,
SYS_ID VARCHAR(8) NOT NULL,
SYS_TYP VARCHAR(1) NOT NULL,
SYS_IP VARCHAR(20),
SYS_PORT VARCHAR(10),
DB_TYP VARCHAR(10),
DB_NM VARCHAR(30),
SVR_TYP VARCHAR(10),
SVR_NM VARCHAR(30))
ENGINE=INNODB;
ALTER TABLE SYS_CON_MST ADD CONSTRAINT SYS_CON_MST_PK PRIMARY KEY (CONN_ID, AVL_TO_DY);
ALTER TABLE SYS_CON_MST ADD FOREIGN KEY (SYS_ID) REFERENCES SYS_INF_MST(SYS_ID);

INSERT INTO SYS_CON_MST
VALUES
       (CONCAT('CONN', LPAD(NEXTVAL(CONN_ID_SEQ), 6, 0)),
        SYSDATE(),
        SYSDATE(),
        'Y',
        'SYS00001',
        'D',
        '127.0.0.1',
        '3306',
        'mariadb',
        'projectdb',
        NULL,
        NULL);
        
        
CREATE TABLE BAT_VIS_MST (
                           BAT_SEQ BIGINT AUTO_INCREMENT PRIMARY KEY ,
                           SYS_ID VARCHAR(8) NOT NULL,
                           APL_YN VARCHAR(1),
                           BAT_PROC_ID VARCHAR(20),
                           BAT_PROC_HNM VARCHAR(50),
                           FILE_YN VARCHAR(1),
                           FILE_ID VARCHAR(20),
                           FILE_HNM VARCHAR(50),
                           RES_FILE_SEND_YN VARCHAR(1),
                           RES_FILE_ID VARCHAR(20),
                           FILE_PROC_HOST VARCHAR(20),
                           FILE_PROC_DIR VARCHAR(100),
                           FILE_PROC_RES_DIR VARCHAR(100),
                           FILE_RCV_HOST VARCHAR(20),
                           FILE_RCV_DIR VARCHAR(100),
                           FILE_SND_HOST VARCHAR(20),
                           FILE_SND_DIR VARCHAR(100),
                           REG_ID VARCHAR(8),
                           REG_DT DATETIME,
                           UPD_ID VARCHAR(8),
                           UPD_DT DATETIME
)ENGINE=INNODB;

CREATE TABLE APR_BATSCH_MST (
                              MBRSH_PGM_ID VARCHAR(1) NOT NULL ,
                              MST_BAT_ID VARCHAR(20) NOT NULL ,
                              MST_BAT_NM VARCHAR(50),
                              HOST_NM VARCHAR(20),
                              INPUT_FILE_YN VARCHAR(1),
                              FILE_ID VARCHAR(10),
                              HNM_FILE_NM VARCHAR(40),
                              PRE_BAT_EXIST_YN VARCHAR(1),
                              FILE_AUTO_SEND_YN VARCHAR(1),
                              MULTI_PROC_CNT BIGINT,
                              REPORT_TYP VARCHAR(1),
                              APL_YN VARCHAR(1),
                              REGR_ID VARCHAR(8),
                              REG_DT DATETIME,
                              UPDR_ID VARCHAR(8),
                              UPD_DT DATETIME
)ENGINE=INNODB;

CREATE TABLE TBL_MAP_MST (
                           CONN_ID VARCHAR(10) NOT NULL ,
                           MAP_TYP VARCHAR(1),
                           TGT_TBL_NM VARCHAR(40),
                           SRC_TBL_NM VARCHAR(40),
                           TGT_COL_NM VARCHAR(40),
                           SRC_COL_NM VARCHAR(40)
)ENGINE=INNODB;        








```



_ _ _


*출처 : 
- <https://mariadb.com/kb/en/library> 
참고