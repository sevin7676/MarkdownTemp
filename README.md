```C#
 #region query data using dbItem class
  //example 1: use apDbItem to select data (also using some other shorthand methods for parameterization and enums for column names)
  var SqlParams = UT.SPC();
  var workFlows = apDbItem.Select<dbWorkflow>(
      Columns: UT.CS(Pn103.WorkflowClassID, Pn103.AssignedTo_UserAccountKey, Pn103.ItemID),
      Where: SqlParams.A(Pn103.NC_CreatedDate, "<", DateTime.Now.AddDays(-7)));


  //example 2: traditional query that does the same thing as above
  var query = string.Format("SELECT WorkflowClassID, AssignedTo_UserAccountKey, ItemID FROM WorkFlow WHERE CreatedDate < '{0}'", DateTime.Now.AddDays(-7));
  workFlows = SM.DataTbl(query).ToDbItem<dbWorkflow>();


  //now we can work with the resulting data with strongly typed properites
  //      hint: if we were querying 1 item, use: workFlows.First()
  foreach (var w in workFlows)
  {
      string r = "data: "
          + w.WorkflowClassID
          + w.AssignedTo_UserAccountKey
          + w.ItemID;
      //using a field that was not assigned will throw an error, example: workFlow.ModifiedDate [error]
  }
  #endregion


  #region insert data using dbItem class
  var workFlow = new dbWorkflow();
  workFlow.WorkflowClassID = 1;
  workFlow.AssignedTo_UserAccountKey = SM.UserAccountKey_FromCookie();
  workFlow.ItemID = 3;
  workFlow.Insert();//returns newly inserted key
  #endregion


  #region update data using dbItem class
  //example 1: update using primary key field
  workFlow = new dbWorkflow();
  workFlow.WorkflowID = 0;//pk
  workFlow.AssignedTo_UserAccountKey = 7;
  workFlow.Update();//returns rows affected


  //example 2: update using any where clause (paramterization is available)
  workFlow = new dbWorkflow();
  workFlow.CreatedDate = DateTime.Now;
  workFlow.Update(WhereClause: Pn103.AssignedTo_UserAccountKey+"=1");//returns rows affected
  #endregion
```
