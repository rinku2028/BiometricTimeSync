# BiometricTimeSync
ZKTeco Biometric Time Sync Automatically

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ComponentModel;
using System.Data;
using System.Threading;
using System.Xml;

namespace Timesyncbiometric
{
    class Program
    {
                      
        public static void Main(string[] args)
        {

            DataSet ds = new DataSet();
            ds.ReadXml("Listofipaddress.xml");

            foreach (DataRow dr in ds.Tables[0].Rows)
            {
                Console.WriteLine(dr[0].ToString());
                ConnectHeadOffice(dr[0].ToString());
            }
         
            Console.ReadLine();

            
           
        }

        public static void ConnectIP(string ip)
        {
            zkemkeeper.CZKEM axCZKEM1 = new zkemkeeper.CZKEM();

            bool bIsConnected = axCZKEM1.Connect_Net(ip, 4370);  // 4370 is port no of attendance machine
            // try
            // {
            if (bIsConnected == true)
            {

                Console.WriteLine("Device connected successfully");
                Console.WriteLine(bIsConnected);
                // Console.ReadLine();
            }
            else
            {
                Console.WriteLine("Device Not Connect");
                //Console.ReadLine();
            }


        }

        public static void ConnectHeadOffice(string ip)
    {
        zkemkeeper.CZKEM axCZKEM1 = new zkemkeeper.CZKEM();
        bool bIsConnected = axCZKEM1.Connect_Net(ip, 4370);
        int iMachineNumber = 1;
        int idwErrorCode = 0;

                    
        if (bIsConnected == false)
        {
                Console.Write("Please connect the device first");
                return;
        }
            
        

            //Cursor = Cursors.WaitCursor;
         if (axCZKEM1.SetDeviceTime(iMachineNumber))
         {
                axCZKEM1.RefreshData(iMachineNumber);//the data in the device should be refreshed
                Console.Write("Successfully set the time of the machine and the terminal to sync PC!");
                int idwYear = 0;
                int idwMonth = 0;
                int idwDay = 0;
                int idwHour = 0;
                int idwMinute = 0;
                int idwSecond = 0;
          if (axCZKEM1.GetDeviceTime(iMachineNumber, ref idwYear, ref idwMonth, ref idwDay, ref idwHour, ref idwMinute, ref idwSecond))//show the time
           {
                   //txtGetDeviceTime.Text = idwYear.ToString() + "-" + idwMonth.ToString() + "-" + idwDay.ToString() + " " + idwHour.ToString() + ":" + idwMinute.ToString() + ":" + idwSecond.ToString();
                
          }

            }
            else
            {
                axCZKEM1.GetLastError(ref idwErrorCode);
                Console.Write("Operation failed,ErrorCode=" + idwErrorCode.ToString());
            }
            // Cursor = Cursors.De
        }

        
    }


 }
        
           
    

