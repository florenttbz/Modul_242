<h1>Modul 242 im BIST Kurs</h1>
Team: Florent Osmani, Shenojan Sathiyamohan, Riccardo Raschetti, Luca Blatter

**Einleitung**

Wir möchten mehrere Sensoren und einen Akteur einbauen, da wir finden die vielfalt macht denn Spass und so wird es uns nicht langweilig beim Programmieren. 

**Testfälle, Problemreports**
![alt text](https://github.com/florenttbz/Modul_242/blob/master/Capture.PNG "Testfälle")

**Hardware**
Model: FRDM-K64F
![alt text](https://github.com/florenttbz/Modul_242/blob/master/ndxmfotro.jpeg "FRDM-K64F")

**Schlusswort**
Das Modul 242 Mikroprozessoranwendung realisieren hat uns sehr viel Freude bereitet.
Das Erarbeiten der einzelnen Aufgabenstellungen und der Leistungsbeurteilung 1 war sehr interessant und herausfordernd.
Die Theorie des Moduls ist nach unserer Meinung eher auf der schwierigeren Seite und weniger interessant. Da dies aber auch dazu gehört haben wir versucht, diese so gut wie möglich zu verstehen, lernen und verwenden.
Als Team sind wir zufrieden mit unserer Leistung.


**Code**
    #include "mbed.h"

    #include "MFRC522.h"



    // NFC/RFID Reader (SPI)

    MFRC522    rfidReader( PTA16, PTC7, PTC5, D10, D8 );



    DigitalOut led1( D13 );


    DigitalOut mosfet( D5 );
    DigitalOut buzzer( D2 );
    DigitalOut buzzer1( D3 );





    PwmOut green( D5 );
    PwmOut red( D6 );
    PwmOut blue( D7 );
    PwmOut ledled ( A4 );
    PwmOut ledled1 ( A5 );
    PwmOut ledled2 ( D8 );
    PwmOut ledled3 ( D9 );







    // erlaubte RFID Tag's

    char ids[3] [4] = {

        { 0x14, 0x63, 0x19, 0x1d },

        { 0x14, 0x63, 0x19, 0x1e },

        { 0x14, 0x63, 0x19, 0x1f },

    };



    int main(void)

    {

        printf( "Init RFID \n" );

        // Init. RC522 Chip

        rfidReader.PCD_Init();

        printf( "bereit\n" );



        while (true) 

        {

            led1 = 0;
            blue = 0;
            red = 0;
            green = 0;
            buzzer = 0;
            buzzer1 = 0;
            ledled = 0;
            ledled1 = 0;
            ledled2 = 0;
            ledled3 = 0;




            // Look for new cards

            if ( rfidReader.PICC_IsNewCardPresent())

                if ( rfidReader.PICC_ReadCardSerial()) 

                {

                    led1   = 1;
                    blue = 1;
                    red = 1;
                    green = 1;
                    buzzer = 1;
                    buzzer1 = 1;
                    ledled = 1;
                    ledled1 = 1;
                    ledled2 = 1;
                    ledled3 = 1; 









                    // Print Card UID (2-stellig mit Vornullen, Hexadecimal)

                    printf("Card UID: ");

                    for ( int i = 0; i < rfidReader.uid.size; i++ )

                        printf("%02X:", rfidReader.uid.uidByte[i]);

                    printf("\n");



                    // alle ids durchlaufen (r = ids, c = position)

                    for ( int r = 0; r < 3; r++ ) 

                    {

                        int ok = true;

                        for ( int c = 0; c < 4; c++ ) 

                        {

                            if  ( rfidReader.uid.uidByte[c] != ids[r] [c] ) 

                            {

                                ok = false;

                                break;

                            }

                        }

                        // RFID Tag's erkannt?

                        if  ( ok ) 

                        {

                            mosfet = 1;

                            wait_ms(1000);

                            mosfet = 0;

                            break;

                        }

                    }

                }

            wait ( 0.2f );

        }



    }
