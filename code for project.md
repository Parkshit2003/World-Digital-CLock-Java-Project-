
// Main java project "World Digital Clock"
// Group members: Parkshit Sharma (12110699), Mhommed Sakeel (0000000), wasim Khan (0000000)




import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.TimeZone;
import java.util.ArrayList;
import java.util.Arrays;


public class WorldClock extends JFrame {
    JLabel timeLabel;
    JComboBox<String> countryList;
    JTextField searchField;

    public WorldClock() {
        setTitle("World Digital Clock");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(300, 150);
        setResizable(true);
        setLocationRelativeTo(null);

        JPanel panel = new JPanel(new GridLayout(3, 3));

       

    searchField = new JTextField("search the TimeZone");
    
        
       searchField.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent f)
    
    {
        String search = searchField.getText().toLowerCase();
        ArrayList<String> filteredItems = new ArrayList<>();
        for (int i = 0; i < countryList.getItemCount(); i++) {
            String item = countryList.getItemAt(i);
            if (item.toLowerCase().contains(search)) {
                filteredItems.add(item);
            }
        }
        countryList.setModel (new DefaultComboBoxModel<>(filteredItems.toArray(new String[0])));
        countryList.setPopupVisible(true);
    }
});

panel.add(searchField, BorderLayout.NORTH);

        
       
        countryList = new JComboBox<>(TimeZone.getAvailableIDs());
        countryList.setSelectedItem(TimeZone.getDefault().getID());
        countryList.addActionListener(new ActionListener()
        {
            public void actionPerformed(ActionEvent e) {
                TimeZone tz = TimeZone.getTimeZone((String) countryList.getSelectedItem());
                Calendar calendar = Calendar.getInstance(tz);
                SimpleDateFormat sdf = new SimpleDateFormat("HH:mm:ss");
                sdf.setTimeZone(tz);
                timeLabel.setText(sdf.format(calendar.getTime()));
            }
        });
        panel.add(countryList);

        

        timeLabel = new JLabel();
        timeLabel.setFont(new Font("Arial", Font.PLAIN, 32));
        timeLabel.setHorizontalAlignment(SwingConstants.CENTER);
        panel.add(timeLabel);
 

        add(panel);

        Timer timer = new Timer(1000, event -> {
            TimeZone tz = TimeZone.getTimeZone((String) countryList.getSelectedItem());
            Calendar calendar = Calendar.getInstance(tz);
            SimpleDateFormat sdf = new SimpleDateFormat("HH:mm:ss");
            sdf.setTimeZone(tz);
            timeLabel.setText(sdf.format(calendar.getTime()));
        });
        timer.start();
    }

    public static void main(String[] args) {
        new WorldClock().setVisible(true);
    }
}
