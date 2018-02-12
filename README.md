# GetArroyoValues
This code is designed to communicate with Arroyo Instruments 5240, and write the time stamped values of temperature, voltage, current, and resistance to a CSV file of your choice. Note that this code is set up to communicate to two instruments at once.

def get_arroyotemp(serialdevice):
    serialdevice.write("TEC:T?\r\n")
    response=serialdevice.readline()
    temperature=response.split()[-1] # breakdown the reply into multiple words, and pick the last one that contains temp readings
    # print 'Temperature(degC)'
    return temperature

def get_arroyocurrent(serialdevice):
    serialdevice.write("TEC:ITE?\r\n")
    response=serialdevice.readline()
    current=response.split()[-1] # breakdown the reply into multiple words, and pick the last one that contains temp readings
    # print 'Current(Amp)'
    return current

def get_arroyoresistance(serialdevice):
    serialdevice.write("TEC:R?\r\n")
    response=serialdevice.readline()
    resistance=response.split()[-1] # breakdown the reply into multiple words, and pick the last one that contains temp readings
    # print 'Resistance(ohm)'
    return resistance

def get_arroyovoltage(serialdevice):
    serialdevice.write("TEC:V?\r\n")
    response=serialdevice.readline()
    voltage=response.split()[-1] # breakdown the reply into multiple words, and pick the last one that contains temp readings
    # print 'Voltage(V)'
    return voltage

# Connects to Arroyo Insturments 5240 via RS232-USB interface
ser1=serial.Serial('/dev/tty.usbserial-A101MY1F',
                  baudrate=38400,
                  bytesize=serial.EIGHTBITS,
                  parity=serial.PARITY_NONE,
                  stopbits=serial.STOPBITS_ONE,
                  timeout=10)
ser2=serial.Serial('/dev/tty.usbserial-A800faLe',
                  baudrate=38400,
                  bytesize=serial.EIGHTBITS,
                  parity=serial.PARITY_NONE,
                  stopbits=serial.STOPBITS_ONE,
                  timeout=10)

time.sleep(0.1) # wait so serial comm is initialized

meas = 1
total_meas=3
wait = 1

lasttime=time.time()

print "doing measurement #.{}".format(0)
with open('Desktop/testfile.csv','a') as csvfile:
    filewriter = csv.writer(csvfile)
    filewriter.writerow([time.time(),get_arroyotemp(ser1),get_arroyovoltage(ser1),get_arroyocurrent(ser1),get_arroyoresistance(ser1),get_arroyotemp(ser2),get_arroyovoltage(ser2),get_arroyocurrent(ser2),get_arroyoresistance(ser2)])
for meas in range(total_meas):
    while (time.time()-lasttime)<=wait:
        pass
    lasttime = time.time()
    print "doing measurement #:{}".format(meas+1)
    with open('Desktop/testfile.csv','a') as csvfile:
        filewriter = csv.writer(csvfile)
        filewriter.writerow([time.time(),get_arroyotemp(ser1),get_arroyovoltage(ser1),get_arroyocurrent(ser1),get_arroyoresistance(ser1),get_arroyotemp(ser2),get_arroyovoltage(ser2),get_arroyocurrent(ser2),get_arroyoresistance(ser2)])
    meas = meas+1
