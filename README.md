
# java使用储备库

<hr>
<br>
<h3>获取客户端IP</h2>

```java
public static String getUserIP(HttpServletRequest request) {
		String ip = request.getHeader("True-Client-IP");
		if(StringUtil.isNullOrBlank(ip)){
			ip = request.getHeader("x-forwarded-for");
		}
		if (StringUtil.isNullOrBlank(ip) || "unknown".equalsIgnoreCase(ip)) {
			ip = request.getHeader("Proxy-Client-IP");
		} else {
			String[] ips = ip.split(",");
			ip = ips[0];
		}
		if (StringUtil.isNullOrBlank(ip)|| "unknown".equalsIgnoreCase(ip)) {
			ip = request.getHeader("WL-Proxy-Client-IP");
		} else {
			return ip.trim();
		}
		if (StringUtil.isNullOrBlank(ip) || "unknown".equalsIgnoreCase(ip)) {
			ip = request.getRemoteAddr();
		}
		return ip != null ? ip.trim() : "";
	}
```

<br>
<h3>获取当前服务器IP</h2>

```java
public static String getAddress()  {
        String local = null;
        try {
            Enumeration<NetworkInterface> allNetInterfaces = NetworkInterface.getNetworkInterfaces();
            InetAddress ip = null;
            while (allNetInterfaces.hasMoreElements()) {
                NetworkInterface netInterface = (NetworkInterface) allNetInterfaces.nextElement();
                if (netInterface.isLoopback() || netInterface.isVirtual() || !netInterface.isUp()) {
                    continue;
                } else {
                    Enumeration<InetAddress> addresses = netInterface.getInetAddresses();
                    while (addresses.hasMoreElements()) {
                        ip = addresses.nextElement();
                        if (ip != null && ip instanceof Inet4Address) {
                            local = ip.getHostAddress();
                        }
                    }
                }
            }
        }catch (SocketException e) {
            e.printStackTrace();
        }
        return local;
    }
```

<br>
<h3>时间Util</h2>

```java
static SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");


/**
	 * 将时间从源时区转为指定时区
	 * @param time
	 * @param sourceTimeZone
	 * @param toTimeZone
	 * @return
	 */
	public static String dateTransformBetweenTimeZone(String time,DateFormat sourceDateFormat,TimeZone sourceTimeZone,DateFormat toDateFormat,TimeZone toTimeZone) {
		sourceDateFormat.setTimeZone(sourceTimeZone);
		try {
			Date date = sourceDateFormat.parse(time);
			toDateFormat.setTimeZone(toTimeZone);
			return toDateFormat.format(date);
		} catch (ParseException e) {
			return null;
		}
	}


	/**
	 * 获取当年的第一天
	 * @return
	 */
	public static String getCurrYearFirst(){
		Calendar currCal=Calendar.getInstance();
		int currentYear = currCal.get(Calendar.YEAR);
		return format.format(getYearFirst(currentYear));
	}

	/**
	 * 获取当年的最后一天
	 * @return
	 */
	public static String getCurrYearLast(){
		Calendar currCal=Calendar.getInstance();
		int currentYear = currCal.get(Calendar.YEAR);
		return format.format(getYearLast(currentYear));
	}

	/**
	 * 获取某年第一天日期
	 * @param year 年份
	 * @return Date
	 */
	public static Date getYearFirst(int year){
		Calendar calendar = Calendar.getInstance();
		calendar.clear();
		calendar.set(Calendar.YEAR, year);
		Date currYearFirst = calendar.getTime();
		return currYearFirst;
	}

	/**
	 * 获取某年最后一天日期
	 * @param year 年份
	 * @return Date
	 */
	public static Date getYearLast(int year){
		Calendar calendar = Calendar.getInstance();
		calendar.clear();
		calendar.set(Calendar.YEAR, year);
		calendar.roll(Calendar.DAY_OF_YEAR, -1);
		Date currYearLast = calendar.getTime();

		return currYearLast;
	}



	/**
	 *获取当月第一天
	 * @return
	 */
	public static String getCurrMonthFirst(){
		Calendar c = Calendar.getInstance();
		c.add(Calendar.MONTH, 0);
		c.set(Calendar.DAY_OF_MONTH,1);
		String first = format.format(c.getTime());
		try {
			return format.format(format.parse(first));
		} catch (ParseException e) {
			return null;
		}
	}

	/**
	 * 获取当月的最后一天
	 * @return
	 */
	public static String getCurrMonthLast(){
		Calendar ca = Calendar.getInstance();
		ca.set(Calendar.DAY_OF_MONTH, ca.getActualMaximum(Calendar.DAY_OF_MONTH));
		String last = format.format(ca.getTime());
		try {
			return format.format(format.parse(last));
		} catch (ParseException e) {
			return null;
		}
	}
```

<br>
<h3>BigDecimal转换四舍五入</h2>

```java
Long deviceAmount = *****;
BigDecimal deviceAmountBigDecimal = BigDecimal.valueOf(deviceAmount).setScale(2,BigDecimal.ROUND_HALF_UP);
```

