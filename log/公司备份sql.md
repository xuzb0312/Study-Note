## 密码重置

管理端

```sql
--创建BA.PASS_RESET_MATERIAL_CONFIG表到指定的表空间TS_BA中
create table BA.PASS_RESET_MATERIAL_CONFIG
(
  xtid    VARCHAR2(20) not null,
  clid    VARCHAR2(20) not null,
  clmc    VARCHAR2(100),
  cllx    VARCHAR2(20),
  clsm    VARCHAR2(500),
  wjdx    VARCHAR2(20),
  sfzs    VARCHAR2(3),
  sfbl    VARCHAR2(3),
  wjsl    VARCHAR2(20),
  jbrid   VARCHAR2(20),
  jbrmc   VARCHAR2(50),
  jbsj    DATE,
  zhxgrid VARCHAR2(20),
  zhxgrmc VARCHAR2(50),
  zhxgsj  DATE,
  bz      VARCHAR2(200)
)
tablespace TS_BA;
-- Add comments to the columns 
comment on column BA.PASS_RESET_MATERIAL_CONFIG.xtid
  is '系统ID';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.clid
  is '材料ID';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.clmc
  is '材料名称';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.cllx
  is '材料类型';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.clsm
  is '材料说明';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.wjdx
  is '文件大小';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.sfzs
  is '是否展示';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.sfbl
  is '是否必录';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.wjsl
  is '文件数量';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.jbrid
  is '经办人ID';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.jbrmc
  is '经办人名称';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.jbsj
  is '经办时间';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.zhxgrid
  is '最后修改人ID';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.zhxgrmc
  is '最后修改人名称';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.zhxgsj
  is '最后修改时间';
comment on column BA.PASS_RESET_MATERIAL_CONFIG.bz
  is '备注';
-- Create/Recreate primary, unique and foreign key constraints 
alter table BA.PASS_RESET_MATERIAL_CONFIG
  add constraint PK_PASS_RESET_MATERIAL_CONFIG primary key (XTID,CLID)
  using index 
  tablespace TS_BA;


--创建BA.PASS_RESET_CONFIG表到指定的表空间TS_BA中
create table BA.PASS_RESET_CONFIG
(
  xtid    VARCHAR2(20) not null,
  xtbh    VARCHAR2(20),
  xtmc    VARCHAR2(100),
  xtshqx  VARCHAR2(500),
  jbrid   VARCHAR2(20),
  jbrmc   VARCHAR2(100),
  jbsj    DATE,
  zhxgrid VARCHAR2(20),
  zhxgrmc VARCHAR2(100),
  zhxgsj  DATE,
  bz      VARCHAR2(500)
)
tablespace TS_BA;
-- Add comments to the table 
comment on table BA.PASS_RESET_CONFIG
  is '新建密码重置配置表';
-- Add comments to the columns 
comment on column BA.PASS_RESET_CONFIG.xtid
  is '系统ID';
comment on column BA.PASS_RESET_CONFIG.xtbh
  is '系统编号';
comment on column BA.PASS_RESET_CONFIG.xtmc
  is '系统名称';
comment on column BA.PASS_RESET_CONFIG.xtshqx
  is '系统审核权限';
comment on column BA.PASS_RESET_CONFIG.jbrid
  is '经办人ID';
comment on column BA.PASS_RESET_CONFIG.jbrmc
  is '经办人名称';
comment on column BA.PASS_RESET_CONFIG.jbsj
  is '经办时间';
comment on column BA.PASS_RESET_CONFIG.zhxgrid
  is '最后修改人ID';
comment on column BA.PASS_RESET_CONFIG.zhxgrmc
  is '最后经办人名称';
comment on column BA.PASS_RESET_CONFIG.zhxgsj
  is '最后修改时间';
comment on column BA.PASS_RESET_CONFIG.bz
  is '备注';
-- Create/Recreate primary, unique and foreign key constraints 
alter table BA.PASS_RESET_CONFIG
  add constraint PK_PASS_RESET_CONFIG primary key (XTID)
  using index 
  tablespace TS_BA;

--生成XTID索引
create sequence BA.SEQ_XTID
minvalue 10000000000
maxvalue 99999999999
start with 10000000001
increment by 1
nocache
cycle;
```

考生端

```sql
-- Create table
create table BA.STD_PASS_RESET
(
  ywlsh      VARCHAR2(20) not null,
  ryid       VARCHAR2(20),
  xm         VARCHAR2(40),
  yxzjlx     VARCHAR2(10),
  yxzjhm     VARCHAR2(50),
  sjhm       VARCHAR2(20),
  sjhmsyc    VARCHAR2(36),
  xtid       VARCHAR2(20),
  jbjgid     VARCHAR2(20),
  ywblzt     VARCHAR2(3),
  spzt       VARCHAR2(3),
  spyj       VARCHAR2(300),
  nextywblzt VARCHAR2(3),
  nextspzt   VARCHAR2(3),
  blfs       VARCHAR2(3),
  jbrid      VARCHAR2(20),
  jbrmc      VARCHAR2(50),
  jbsj       DATE,
  zhxgrid    VARCHAR2(20),
  zhxgrmc    VARCHAR2(50),
  zhxgsj     DATE,
  bz         VARCHAR2(500)
)
tablespace TS_BA;
-- Add comments to the table 
comment on table BA.STD_PASS_RESET
  is '密码重置业务表';
-- Add comments to the columns 
comment on column BA.STD_PASS_RESET.ywlsh
  is '业务流水号';
comment on column BA.STD_PASS_RESET.ryid
  is '人员ID';
comment on column BA.STD_PASS_RESET.xm
  is '姓名';
comment on column BA.STD_PASS_RESET.yxzjlx
  is '有效证件类型';
comment on column BA.STD_PASS_RESET.yxzjhm
  is '有效证件号码';
comment on column BA.STD_PASS_RESET.sjhm
  is '手机号码';
comment on column BA.STD_PASS_RESET.sjhmsyc
  is '手机号码索引串';
comment on column BA.STD_PASS_RESET.xtid
  is '系统ID';
comment on column BA.STD_PASS_RESET.jbjgid
  is '经办机构ID';
comment on column BA.STD_PASS_RESET.ywblzt
  is '业务办理状态';
comment on column BA.STD_PASS_RESET.spzt
  is '审批状态';
comment on column BA.STD_PASS_RESET.spyj
  is '审批意见';
comment on column BA.STD_PASS_RESET.nextywblzt
  is '下一个业务办理状态';
comment on column BA.STD_PASS_RESET.nextspzt
  is '下一个审批状态';
comment on column BA.STD_PASS_RESET.blfs
  is '办理方式';
comment on column BA.STD_PASS_RESET.jbrid
  is '经办人ID';
comment on column BA.STD_PASS_RESET.jbrmc
  is '经办人名称';
comment on column BA.STD_PASS_RESET.jbsj
  is '经办时间';
comment on column BA.STD_PASS_RESET.zhxgrid
  is '最后修改人ID';
comment on column BA.STD_PASS_RESET.zhxgrmc
  is '最后修改人名称';
comment on column BA.STD_PASS_RESET.zhxgsj
  is '最后修改时间';
comment on column BA.STD_PASS_RESET.bz
  is '备注';
-- Create/Recreate primary, unique and foreign key constraints 
alter table BA.STD_PASS_RESET
  add constraint PK_STD_PASS_RESET primary key (YWLSH)
  using index 
  tablespace TS_BA;

--向bi.func_config表中添加一个记录
INSERT INTO BI.FUNC_CONFIG 
SELECT DBID,'std28','密码重置','stdroot','stdrg.do?method=fwdPasswordResetPage','B','student_kqccjg.png','','26','PC','student_kqccjgsh.png' FROM FW.DBID_INFO

```

审核端

```sql
--功能菜单
INSERT INTO FW.FUNC
(DBID, GNID, GNMC, FGN, GNSJ, GNLX, SXH, GNTB, BZ, YWLB)
SELECT DBID,'bar010h', '密码重置', 'bar01', NULL, 'C', 12, NULL, NULL, 'bas' FROM FW.DBID_INFO
--功能按钮  密码重置受理
INSERT INTO FW.FUNC
(DBID, GNID, GNMC, FGN, GNSJ, GNLX, SXH, GNTB, BZ, YWLB)
SELECT DBID, 'bar010h01', '密码重置受理', 'bar010h', 'url:pwd.do?method=fwdPasswordResetSlPage', 'C', 0, 'icon-user-suit', NULL, 'bas'  FROM FW.DBID_INFO
--功能按钮  密码重置受理
INSERT INTO FW.FUNC
(DBID, GNID, GNMC, FGN, GNSJ, GNLX, SXH, GNTB, BZ, YWLB)
SELECT DBID,'bar010h02', '密码重置办理', 'bar010h', 'url:pwd.do?method=fwdPasswordResetCLPage', 'C', 1, 'icon-user-red', NULL, 'bas' FROM FW.DBID_INFO
--功能按钮  密码重置办结
INSERT INTO FW.FUNC
(DBID, GNID, GNMC, FGN, GNSJ, GNLX, SXH, GNTB, BZ, YWLB)
SELECT DBID, 'bar010h03', '密码重置办结', 'bar010h', 'url:pwd.do?method=fwdPasswordResetBJPage', 'C', 2, 'icon-user-orange', NULL, 'bas' FROM FW.DBID_INFO


INSERT INTO FW.FUNC_DOC
(GNID, GNMC, FGN, GNSJ, GNLX, GNTB, BZ)
VALUES('bar010h', '密码重置', 'bar01', NULL, 'C', NULL, NULL);

INSERT INTO FW.FUNC_DOC
(GNID, GNMC, FGN, GNSJ, GNLX, GNTB, BZ)
VALUES('bar010h01', '密码重置受理', 'bar010h', 'url:pwd.do?method=fwdPasswordResetSlPage', 'C', 'icon-user-suit', NULL);

INSERT INTO FW.FUNC_DOC
(GNID, GNMC, FGN, GNSJ, GNLX, GNTB, BZ)
VALUES('bar010h02', '密码重置办理', 'bar010h', 'url:pwd.do?method=fwdPasswordResetCLPage', 'C', 'icon-user-red', NULL);

INSERT INTO FW.FUNC_DOC
(GNID, GNMC, FGN, GNSJ, GNLX, GNTB, BZ)
VALUES('bar010h03', '密码重置办结', 'bar010h', 'url:pwd.do?method=fwdPasswordResetBJPage', 'C', 'icon-user-orange', NULL);



INSERT INTO FW.FUNC_UNION
(DBID, GNID, GNMC, JBJGFW, BZ)
SELECT DBID, 'bar010h', '密码重置', '*', NULL FROM FW.DBID_INFO

INSERT INTO FW.FUNC_UNION
(DBID, GNID, GNMC, JBJGFW, BZ)
SELECT DBID, 'bar010h01', '密码重置受理', '*', NULL FROM FW.DBID_INFO

INSERT INTO FW.FUNC_UNION
(DBID, GNID, GNMC, JBJGFW, BZ)
SELECT DBID, 'bar010h02', '密码重置办理', '*', NULL FROM FW.DBID_INFO

INSERT INTO FW.FUNC_UNION
(DBID, GNID, GNMC, JBJGFW, BZ)
SELECT DBID, 'bar010h03', '密码重置办结', '*', NULL FROM FW.DBID_INFO

--在BA.APV_CONFIG表中添加密码重置审核状态信息
INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '01', NULL, '01', '00');

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '01', '30', NULL, NULL);

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '01', '21', NULL, NULL);

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '01', '20', NULL, NULL);

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '01', '10', '02', '00');

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '02', NULL, '02', '00');

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '02', '20', NULL, NULL);

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '02', '10', '03', '00');

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '02', '21', NULL, NULL);

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '03', NULL, '03', '00');

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '03', '30', NULL, NULL);

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '03', '21', NULL, NULL);

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '03', '20', NULL, NULL);

INSERT INTO BA.APV_CONFIG
(YWLX, CZQYWBLZT, CZQSPZT, CZHYWBLZT, CZHSPZT)
VALUES('MMCZ', '03', '10', NULL, NULL);
```

