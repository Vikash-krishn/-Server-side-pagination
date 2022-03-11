


Pagination in Servlet
To divide large number of records into multiple parts, we use pagination. It allows user to display a part of records only. Loading all records in a single page may take time, so it is always recommended to created pagination. In servlet, we can develop pagination example easily.

In this servlet pagination example, we are using MySQL database to fetch records.

Here, we have created "emp" table in "test" database. The emp table has three fields: id, name and salary. Either create table and insert records manually or import our sql file---


index.html
<a href="ViewServlet?page=1">View Employees</a>  
ViewServlet.java
package com.javatpoint.servlets;  
  
import java.io.IOException;  
import java.io.PrintWriter;  
import java.util.List;  
import javax.servlet.ServletException;  
import javax.servlet.annotation.WebServlet;  
import javax.servlet.http.HttpServlet;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import com.javatpoint.beans.Emp;  
import com.javatpoint.dao.EmpDao;  
  
@WebServlet("/ViewServlet")  
public class ViewServlet extends HttpServlet {  
    private static final long serialVersionUID = 1L;  
    protected void doGet(HttpServletRequest request, HttpServletResponse response)   
           throws ServletException, IOException {  
        response.setContentType("text/html");  
        PrintWriter out=response.getWriter();  
          
        String spageid=request.getParameter("page");  
        int pageid=Integer.parseInt(spageid);  
        int total=5;  
        if(pageid==1){}  
        else{  
            pageidpageid=pageid-1;  
            pageidpageid=pageid*total+1;  
        }  
        List<Emp> list=EmpDao.getRecords(pageid,total);  
  
        out.print("<h1>Page No: "+spageid+"</h1>");  
        out.print("<table border='1' cellpadding='4' width='60%'>");  
        out.print("<tr><th>Id</th><th>Name</th><th>Salary</th>");  
        for(Emp e:list){  
            out.print("<tr><td>"+e.getId()+"</td><td>"+e.getName()+"</td><td>"+e.getSalary()+"</td></tr>");  
        }  
        out.print("</table>");  
          
        out.print("<a href='ViewServlet?page=1'>1</a> ");  
        out.print("<a href='ViewServlet?page=2'>2</a> ");  
        out.print("<a href='ViewServlet?page=3'>3</a> ");  
          
        out.close();  
    }  
}  
Emp.java

package com.javatpoint.beans;  
  
public class Emp {  
private int id;  
private String name;  
private float salary;  
//getters and setters  
}  
EmpDao.java

package com.javatpoint.dao;  
import com.javatpoint.beans.*;  
import java.sql.*;  
import java.util.ArrayList;  
import java.util.List;  
public class EmpDao {  
  
    public static Connection getConnection(){  
        Connection con=null;  
        try{  
            Class.forName("com.mysql.jdbc.Driver");  
            con=DriverManager.getConnection("jdbc:mysql://localhost:3306/test","","");  
        }catch(Exception e){System.out.println(e);}  
        return con;  
    }  
  
    public static List<Emp> getRecords(int start,int total){  
        List<Emp> list=new ArrayList<Emp>();  
        try{  
            Connection con=getConnection();  
            PreparedStatement ps=con.prepareStatement("select * from emp limit "+(start-1)+","+total);  
            ResultSet rs=ps.executeQuery();  
            while(rs.next()){  
                Emp e=new Emp();  
                e.setId(rs.getInt(1));  
                e.setName(rs.getString(2));  
                e.setSalary(rs.getFloat(3));  
                list.add(e);  
            }  
            con.close();  
        }catch(Exception e){System.out.println(e);}  
        return list;  
    }  
}  
 





    

