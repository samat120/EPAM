Load Restaurant Data

~~~scala
// ================= RESTAURANTS =================

val restaurantsDs = spark.read
  .option("header", "true")
  .option("inferSchema", "true")
  .csv("src/main/resources/restaurant_csv")
  .select(
    $"id",
    $"franchise_id",
    $"franchise_name",
    $"restaurant_franchise_id",
    $"country",
    $"city",
    $"lat",
    $"lng"
  )
  .as[(Long, Int, String, Int, String, String, Option[Double], Option[Double])]
  .map {
    case (id, fid, fname, rfid, country, city, lat, lng) =>
      Restaurant(id, fid, fname, rfid, country, city, lat, lng, None)
  }

println(
  s"NULL coordinates BEFORE: " +
    restaurantsDs.filter(r => r.lat.isEmpty || r.lng.isEmpty).count()
)
~~~
