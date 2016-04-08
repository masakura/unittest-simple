### ソースコード

料金の計算をする部分はこんな感じ。

```
protected void Calculate_Click(object sender, EventArgs e)
{
    using (var connection = CreateConnection())
    {
        connection.Open();

        using (var command = connection.CreateCommand())
        {
            var videoId = int.Parse(VideoList.SelectedValue);
            command.CommandText = "select Type from Videos where Id = @Id";
            var idParamter = command.CreateParameter();
            idParamter.ParameterName = "Id";
            idParamter.DbType = DbType.UInt32;
            idParamter.Value = videoId;
            command.Parameters.Add(idParamter);

            using (var reader = command.ExecuteReader())
            {
                if (reader.Read())
                {
                    var type = (VideoType) reader.GetInt32(0);
                    var number = int.Parse(Number.Text);

                    switch (type)
                    {
                        case VideoType.Normal:
                            if (number <= 7)
                            {
                                Price.Text = (7*500 + (number - 7)*300).ToString();
                            }
                            else
                            {
                                Price.Text = (number*500).ToString();
                            }
                            break;

                        case VideoType.New:
                            Price.Text = (number*500).ToString();
                            break;

                        case VideoType.Old:
                            if (number <= 7)
                            {
                                Price.Text = 500.ToString();
                            }
                            else
                            {
                                Price.Text = (500 + (number - 7)*300).ToString();
                            }
                            break;

                        case VideoType.Kids:
                            if (number <= 3)
                            {
                                Price.Text = 300.ToString();
                            }
                            else
                            {
                                Price.Text = (300 + (number - 3)*100).ToString();
                            }
                            break;

                        default:
                            throw new InvalidOperationException();
                    }
                }
            }
        }
    }
}
```
