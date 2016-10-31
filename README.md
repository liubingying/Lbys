## 查询用户test1可以查看的页面
sql语句如下  <br/>
SELECT cp.PrivilegeMaster AS '角色类型',   <br/>
    cp.PrivilegeMasterKey AS '类型编号',   <br/>
    cp.PrivilegeAccess AS '对象类型',   <br/>
    cp.PrivilegeAccessKey AS '对象编号',   <br/>
    sm.MenuName AS '菜单名称'   <br/>
 FROM cf_privilege AS cp   <br/>
  LEFT JOIN sys_menu AS sm ON cp.PrivilegeAccessKey = sm.MenuID AND cp.PrivilegeAccess = 'Sys_Menu' <br/>
 WHERE ((cp.PrivilegeMaster = 'CF_Role' <br/>
     AND cp.PrivilegeMasterKey    
     IN (SELECT RoleID FROM cf_userrole AS cur
       LEFT JOIN cf_user AS cu ON cur.UserID = cu.UserID WHERE cu.LoginName='test1' )) <br/>
    OR <br/>
     (cp.PrivilegeMaster = 'CF_User' <br/>
     AND cp.PrivilegeMasterKey = (SELECT UserID FROM cf_user WHERE LoginName = 'test1'))) <br/>
    AND <br/>
     cp.PrivilegeOperation = 'Permit' AND cp.PrivilegeAccess = 'Sys_Menu'; <br/>
     伪代码： <br/>
     1、根据名称查找人员编号UserID <br/>
     2、根据人员的编号UserID查找该人员所对应的角色集合RoleIDs <br/>
     3、在权限表LEFT JOIN 页面表中查找角色类型为CF_Role   AND   角色编号在角色集合RoleIDs中 <br/>
  OR  角色类型为CF_User   AND   人员编号为UserID  AND 权限属性为Permit   AND   权限为Sys_Menu的数据 <br/>
   查询结果如图：
   ![image](1.jpg)<br/>
  **查看订单页面中的操作权限**<br/>
  sql语句如下： <br/>
  SELECT cp.PrivilegeMaster AS '角色类型',<br/>
    cp.PrivilegeMasterKey AS '类型编号',<br/>
    cp.PrivilegeAccess AS '对象类型',<br/>
    cp.PrivilegeAccessKey AS '对象编号',<br/>
    sb.BtnName AS '按钮名称'<br/>
 FROM cf_privilege AS cp<br/>
  LEFT JOIN sys_button AS sb ON cp.PrivilegeAccessKey = sb.BtnID AND cp.PrivilegeAccess = 'Sys_Button'<br/>
  LEFT JOIN sys_menu AS sm ON sb.MenuNo = sm.MenuNo<br/>
 WHERE ((cp.PrivilegeMaster = 'CF_Role'<br/>
     AND cp.PrivilegeMasterKey<br/>
     IN (SELECT RoleID FROM cf_userrole AS cur<br/>
       LEFT JOIN cf_user AS cu ON cur.UserID = cu.UserID WHERE cu.LoginName='test1' ))<br/>
    OR<br/>
     (cp.PrivilegeMaster = 'CF_User'<br/>
     AND cp.PrivilegeMasterKey = (SELECT UserID FROM cf_user WHERE LoginName = 'test1')))<br/>
    AND<br/>
     cp.PrivilegeOperation = 'Permit' AND cp.PrivilegeAccess = 'Sys_Button' AND sm.MenuName = '订单';<br/>
     伪代码：<br/>
   1、根据名称查找人员编号UserID<br/>
   2、根据人员编号UserID查找改人员所对应的角色集合RoleIDs<br/>
   3、权限表 LEFT JOIN 按钮表 LEFT JOIN 页面表并查找
   角色类型为CF_Role   AND   角色编号在角色集合RoleIDs中
  OR
   角色类型为CF_User   AND   人员编号为UserID
  AND 权限属性为Permit   AND   权限为Sys_Button AND 菜单名字为订单的数据<br/>
 查询结果如图<br/>
  ![image](2.jpg)
  
