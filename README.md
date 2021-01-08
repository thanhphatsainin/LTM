/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package Control;

import Model.User;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;

/**
 *
 * @author DELL
 */
public class Dao {
    private Connection con;
    private Statement st;
    
    public Dao(){
        conectDB();
    }
    
    public void conectDB(){
        String dbUrl = "jdbc:mysql://localhost:3306/hotel";
        String dbClass = "com.mysql.jdbc.Driver";
        try {
            Class.forName(dbClass);
            con = DriverManager.getConnection(dbUrl, "root", "10101999");
            st = con.createStatement();
            System.out.println("Connect succes!");
        } catch (Exception e) {
        }
    }
    
    public boolean checkLogin(User user){
        String  query = "Select * from tbluser where username = '"+user.getUserName()+"' and password ='"+user.getPassword()+"'";
        System.out.println(query);
        try {
            ResultSet rs = st.executeQuery(query);
            if (rs.next()) {
                return true;
            }
        } catch (Exception e) {
        }
        return false;
    }
    
    public User getUserByUserName(String username){
        String  query = "Select * from tbluser where username = '"+username+"'";
        User u = new User();
        System.out.println(query);
        
        try {
            ResultSet rs = st.executeQuery(query);
            if (rs.next()) {
                u.setId(rs.getInt(1));
                u.setUserName(rs.getString(2));
                u.setPassword(rs.getString(3));
            }
        } catch (Exception e) {
        }
        return u;
    }
    
    public boolean add(User user){
        System.out.println(user.getUserName() + " " + user.getPassword());
        String query = "Insert into tbluser(username, password, name, position) values(?, ?, ?, ?)";
        System.out.println(query);
        try {
            PreparedStatement ps = con.prepareStatement(query, Statement.RETURN_GENERATED_KEYS);
            ps.setString(1, user.getUserName());
            ps.setString(2, user.getPassword());
            ps.setString(3, "no");
            ps.setString(4, "no");
            ps.executeUpdate();
            
            ResultSet rs = ps.getGeneratedKeys();
            if (rs.next()) {
                user.setId(rs.getInt(1));
            }
            return true;
        } catch (Exception e) {
        }
        return false;
    }

	

    
    public boolean edit(User user){
        String  query = "Update tbluser set username = '"+user.getUserName()+"',password ='"+user.getPassword()+"' where id = '"+user.getId()+"'";
        System.out.println(query);
        try {
            if (st.executeUpdate(query) != -1) {
                return true;
            }
        } catch (Exception e) {
        }
        return false;
    }
    
    public void deleteClient(int id){
		String sql = "DELETE FROM tblclient WHERE id=?";
		try{
			PreparedStatement ps = con.prepareStatement(sql);
			ps.setInt(1, id);

			ps.executeUpdate();
		}catch(Exception e){
			e.printStackTrace();
		}
	}
}
