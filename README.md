# MVC_CRUD_Oprations
ASP.Net MVC CRUD oprations


===================================================StudentController=============================================================

using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using System.Web;
using System.Web.Mvc;
using DemoCRUD.Entity;
using DemoCRUD.Models;

namespace DemoCRUD.Controllers
{
    public class StudentController : Controller
    {
        Entity.DemoDbEntities dbentity =new DemoDbEntities();
        // GET: Student
        public ActionResult Index()
        {
            //var mstdetails = from sd in dbentity.StudentDetails
            //    join dep in dbentity.MstDepartments on sd.DepartmentID equals dep.DepartmentID
            //    join gen in dbentity.MstGenders on sd.GenderID equals gen.GenderID
            //    select new
            //    {
            //        sd.StudentID,sd.Name,sd.Email,dep.DepartmentName,gen.GenderName
            //    };
            return View(dbentity.StudentDetails.ToList());
        }

        // GET: Student/Details/5
        public ActionResult Details(int id)
        {
            var std = dbentity.StudentDetails.Where(s => s.StudentID == id).FirstOrDefault();
            return View(std);
        }

        // GET: Student/Create
        public ActionResult Create()
        {
            ViewBag.DepList = new SelectList(dbentity.MstDepartments, "DepartmentID", "DepartmentName");

            ViewBag.GenList = new SelectList(dbentity.MstGenders, "GenderID", "GenderName");
            return View();
        }

        // POST: Student/Create
        [HttpPost]
        public ActionResult Create(StudentDetail collection)
        {
            try
            {                
                StudentDetail sd =new StudentDetail();
                // TODO: Add insert logic here
                sd.Name = collection.Name;
                sd.Email = collection.Email;
                sd.DepartmentID = collection.DepartmentID;
                sd.GenderID = collection.GenderID;
                dbentity.StudentDetails.Add(sd);
                dbentity.SaveChanges();
            }
            catch(Exception e)
            {
                throw e;
                
            }
            return RedirectToAction("Index", "Student");
        }

        // GET: Student/Edit/5
        public ActionResult Edit(int id)
        {
            ViewBag.DepList = new SelectList(dbentity.MstDepartments, "DepartmentID", "DepartmentName");

            ViewBag.GenList = new SelectList(dbentity.MstGenders, "GenderID", "GenderName");
            //StudentDetail sd = new StudentDetail();
            var std = dbentity.StudentDetails.Where(s => s.StudentID == id).FirstOrDefault();
            return View(std);
        }

        // POST: Student/Edit/5
        [HttpPost]
        public ActionResult Edit(int id, StudentDetail collection)
        {
            ViewBag.DepList = new SelectList(dbentity.MstDepartments, "DepartmentID", "DepartmentName");
            ViewBag.GenList = new SelectList(dbentity.MstGenders, "GenderID", "GenderName");

            try
            {
                StudentDetail ObjData = dbentity.StudentDetails.Where(x => x.StudentID == id).Select(x => x).FirstOrDefault();
                //StudentDetail sd = new StudentDetail();
                // TODO: Add update logic here
                ObjData.Name = collection.Name;
                ObjData.Email = collection.Email;
                ObjData.DepartmentID = collection.DepartmentID;
                ObjData.GenderID = collection.GenderID;
                //dbentity.StudentDetails.
                dbentity.SaveChanges();
               
                return RedirectToAction("Index");
            }
            catch
            {
                return View();
            }
        }

        // GET: Student/Delete/5
        public ActionResult Delete(int id)
        {
            ViewBag.DepList = new SelectList(dbentity.MstDepartments, "DepartmentID", "DepartmentName");
            ViewBag.GenList = new SelectList(dbentity.MstGenders, "GenderID", "GenderName");

            var std = dbentity.StudentDetails.Where(s => s.StudentID == id).FirstOrDefault();
            dbentity.StudentDetails.Remove(std);
            dbentity.SaveChanges();
            return RedirectToAction("Index");
        }

        // POST: Student/Delete/5
        //[HttpPost]
        //public ActionResult Delete(int id, StudentDetail collection)
        //{
        //    try
        //    {
        //        // TODO: Add delete logic here
        //        StudentDetail obj =dbentity.StudentDetails.Where(x => x.StudentID == id).Select(x => x).FirstOrDefault();
        //        dbentity.SaveChanges();
        //        return RedirectToAction("Index");
        //    }
        //    catch
        //    {
        //        return View();
        //    }
        //}
    }
}


===========================================================Models classes=============================================================================
Edmx file
===============
using System.ComponentModel.DataAnnotations;

namespace DemoCRUD.Entity
{
    using System;
    using System.Collections.Generic;
    
    public partial class StudentDetail
    {
        [Display(Name="Student ID")]
        public int StudentID { get; set; }
        [Display(Name = "Name")]
        public string Name { get; set; }
        public string Email { get; set; }
        [Display(Name = "Department")]
        public int? DepartmentID { get; set; }
        [Display(Name = "Gender")]
        public int? GenderID { get; set; }
    
        public virtual MstDepartment MstDepartment { get; set; }
        public virtual MstGender MstGender { get; set; }
    }
}

namespace DemoCRUD.Entity
{
    using System;
    using System.Collections.Generic;
    
    public partial class MstDepartment
    {
        [System.Diagnostics.CodeAnalysis.SuppressMessage("Microsoft.Usage", "CA2214:DoNotCallOverridableMethodsInConstructors")]
        public MstDepartment()
        {
            this.StudentDetails = new HashSet<StudentDetail>();
        }
    
        public int DepartmentID { get; set; }
        public string DepartmentName { get; set; }
    
        [System.Diagnostics.CodeAnalysis.SuppressMessage("Microsoft.Usage", "CA2227:CollectionPropertiesShouldBeReadOnly")]
        public virtual ICollection<StudentDetail> StudentDetails { get; set; }
    }
}


namespace DemoCRUD.Entity
{
    using System;
    using System.Collections.Generic;
    
    public partial class MstGender
    {
        [System.Diagnostics.CodeAnalysis.SuppressMessage("Microsoft.Usage", "CA2214:DoNotCallOverridableMethodsInConstructors")]
        public MstGender()
        {
            this.StudentDetails = new HashSet<StudentDetail>();
        }
    
        public int GenderID { get; set; }
        public string GenderName { get; set; }
    
        [System.Diagnostics.CodeAnalysis.SuppressMessage("Microsoft.Usage", "CA2227:CollectionPropertiesShouldBeReadOnly")]
        public virtual ICollection<StudentDetail> StudentDetails { get; set; }
    }
}


================cshtml files================
Index.cshtml  list render file

@model IEnumerable<DemoCRUD.Entity.StudentDetail>

@{
    ViewBag.Title = "Index";
}

<h2>Index</h2>
<link href="~/Content/bootstrap.min.css" rel="stylesheet" />
<p>
    @Html.ActionLink("Create New", "Create")
</p>
<table class="table">
    <tr>
        @*<th>
            @Html.DisplayNameFor(model => model.StudentID)
        </th>*@
        <th>
            @Html.DisplayNameFor(model => model.Name)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Email)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.DepartmentID)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.GenderID)
        </th>
        <th>
            Options
        </th>
    </tr>

@foreach (var item in Model) {
    <tr>
        @*<td>
            @Html.DisplayFor(modelItem => item.StudentID)
        </td>*@
        <td>
            @Html.DisplayFor(modelItem => item.Name)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.Email)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.MstDepartment.DepartmentName)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.MstGender.GenderName)
        </td>
        <td>
            @Html.ActionLink("Edit", "Edit", new {  id=item.StudentID }) |
            @Html.ActionLink("Details", "Details", new { id = item.StudentID }) |
            @Html.ActionLink("Delete", "Delete", new { id = item.StudentID })
        </td>
    </tr>
}

</table>

=======================
Edit.cshtml
==========
@model DemoCRUD.Entity.StudentDetail

@{
    ViewBag.Title = "Edit";
}

<h2>Edit</h2>

@using (Html.BeginForm("Edit","Student",FormMethod.Post))
{
    @Html.AntiForgeryToken()
    
    <div class="form-horizontal">
        <h4>StudentDetail</h4>
        <hr />
        @Html.ValidationSummary(true, "", new { @class = "text-danger" })
        @*<div class="form-group">
            @Html.LabelFor(model => model.StudentID, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.StudentID, new { htmlAttributes = new { @class = "form-control read" , @readonly = true } })
                @Html.ValidationMessageFor(model => model.StudentID, "", new { @class = "text-danger" })
            </div>
        </div>*@

        <div class="form-group">
            @Html.LabelFor(model => model.Name, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.Name, new { htmlAttributes = new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.Name, "", new { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            @Html.LabelFor(model => model.Email, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.Email, new { htmlAttributes = new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.Email, "", new { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            @Html.LabelFor(model => model.DepartmentID, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">                
                @Html.DropDownListFor(model => model.DepartmentID, ViewBag.DepList as SelectList, "--Select Department--", new { @class = "form-control" })
                @*@Html.EditorFor(model => model.MstDepartment.DepartmentName, new { htmlAttributes = new { @class = "form-control" } })*@                
                @Html.ValidationMessageFor(model => model.MstDepartment.DepartmentName, "", new { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            @Html.LabelFor(model => model.GenderID, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.DropDownListFor(model => model.GenderID, ViewBag.GenList as SelectList, "--Select Department--", new { @class = "form-control" })
                @*@Html.EditorFor(model => model.MstGender.GenderName, new { htmlAttributes = new { @class = "form-control" } })*@
                @Html.ValidationMessageFor(model => model.MstGender.GenderName, "", new { @class = "text-danger" })
            </div>
        </div>

        
        <div class="form-group">
            <div class="col-md-offset-2 col-md-10">
                <input type="submit" value="Save" class="btn btn-default" />
            </div>
        </div>
    </div>
}

<div>
    @Html.ActionLink("Back to List", "Index")
</div>

@section Scripts {
    @Scripts.Render("~/bundles/jqueryval")
}


=================================================================

Create.cshtml
===============
@using DemoCRUD.Models
@using Microsoft.Ajax.Utilities
@*@model DemoCRUD.Models.StudentDetails*@
@model DemoCRUD.Entity.StudentDetail


@{
    ViewBag.Title = "Create";
}

<h2>Create</h2>


@using (Html.BeginForm("Create", "Student", FormMethod.Post))
{
    @Html.AntiForgeryToken()

    <div class="form-horizontal">
        <h4>StudentDetails</h4>
        <hr />
        @Html.ValidationSummary(true, "", new { @class = "text-danger" })
        <div class="form-group">
            @Html.LabelFor(model => model.Name, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.Name, new { htmlAttributes = new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.Name, "", new { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            @Html.LabelFor(model => model.Email, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.Email, new { htmlAttributes = new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.Email, "", new { @class = "text-danger" })
            </div>
        </div>
        <div class="form-group">
            @Html.LabelFor(model => model.DepartmentID, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @*@Html.DropDownList("DepList", "Department" )*@
                @*@Html.DropDownList("DepList", null, "Department", new { @class = "form-control" })*@
                @Html.DropDownListFor(model => model.DepartmentID, ViewBag.DepList as SelectList, "--Select Department--", new { @class = "form-control" })

            </div>
        </div>
        <div class="form-group">
            @Html.LabelFor(model => model.GenderID, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.DropDownListFor(model => model.GenderID, ViewBag.GenList as SelectList,"--Select Gender--", new { @class = "form-control" } )
            </div>
        </div>

        <div class="form-group">
            <div class="col-md-offset-2 col-md-10">
                <input type="submit" value="Create" class="btn btn-default" />
            </div>
        </div>
    </div>
}

<div>
    @Html.ActionLink("Back to List", "Index")
</div>

@section Scripts {
    @Scripts.Render("~/bundles/jqueryval")
}



========================================
Detals.cshtml

================
@model DemoCRUD.Entity.StudentDetail

@{
    ViewBag.Title = "Details";
}

<h2>Details</h2>

<div>
    <h4>StudentDetail</h4>
    <hr />
    <dl class="dl-horizontal">
        @*<dt>
            @Html.HiddenFor(model => model.StudentID)
        </dt>

        <dd>
            @Html.HiddenFor(model => model.StudentID)*@
        </dd>

        <dt>
            @Html.DisplayNameFor(model => model.Name)
        </dt>

        <dd>
            @Html.DisplayFor(model => model.Name)
        </dd>

        <dt>
            @Html.DisplayNameFor(model => model.Email)
        </dt>

        <dd>
            @Html.DisplayFor(model => model.Email)
        </dd>

        <dt>
            @Html.DisplayNameFor(model => model.DepartmentID)
        </dt>

        <dd>
            @Html.DisplayFor(model => model.MstDepartment.DepartmentName)
        </dd>

        <dt>
            @Html.DisplayNameFor(model => model.GenderID)
        </dt>

        <dd>
            @Html.DisplayFor(model => model.MstGender.GenderName)
        </dd>

    </dl>
</div>
<p>
    @Html.ActionLink("Edit", "Edit", new {  id = Model.StudentID }) |
    @Html.ActionLink("Back to List", "Index")
</p>
