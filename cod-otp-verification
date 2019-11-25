<script type="text/javascript">
  if(Shopify.Checkout.step == 'payment_method' ){
    // assign cod_payment to value of cod radio button
    {%- assign cod_payment = 22552969329 -%}
    window.otpverified = false;
    // enter the id of cod radio button in window.codInput
    window.codInput = '#checkout_payment_gateway_22552969329';
    simply.verfiyHTML = $("#verify_otp").html();
    simply.otpVerifyAjaxSubmit = function(value,number){
      // enter url of api that verify otp
      var link = "";
      // if verifyId require then set otherwise remove it from data. 
      var id = localStorage.getItem("verifyId");
      var params = {
        type: 'POST',
        url: link,
        data: 'otp=' + value + '&number=' + number + '&u_id='+ id,
        success: function (data) {
          var $data = JSON.parse(data);
          var resp = JSON.parse($data.response);
          if ($data.status == "0") {
           $("#verify_otp .verify_error").text("OTP Verified Successfully..").show();
            window.otpverified = true;
            setTimeout(function(){
              $("#verify_otp").hide();
              $('.step__footer__continue-btn[data-trekkie-id="complete_order_button"]').closest("form").submit();
            },2000);
          }
          else{
            $("#verify_otp .verify_error").text($data.msg).show();
          }
        },
        error: function (XMLHttpRequest, textStatus) {
          $("#verify_otp .verify_error").text("OTP Not Verified! try Again..").show();
        }
      };
      jQuery.ajax(params);
    };

    simply.ajaxSubmit = function (mob_number) {
      // if token is required then send in api otherwise remove it.
      var token = Shopify.Checkout.token;
      var data = {
        number:mob_number,
        checkout_token:token
      };
      // enter api url that send otp to user
      var link = "";
      var params = {
        type: 'POST',
        url: link,
        data: data,
        success: function (data) {
          var $data = JSON.parse(data);
          if ($data.status == "0") {
            var resp = JSON.parse($data.response);
            // if verifyId require then set otherwise remove it
            localStorage.setItem("verifyId",$data.id);
            if (resp.type == 'success') {
              var countdown = 50;
              var countdownNumberEl = $('#countdown-number');
              countdownNumberEl.text(countdown);
              $("#resendotp").prop("disabled",true);
              $("#countdown").show();
              var refreshId = setInterval(function () {
                countdown = countdown - 1;
                countdownNumberEl.text(countdown);
                if (($("#countdown-number").text()) == "0") {
                  clearInterval(refreshId);
                  $("#countdown").hide();
                  $("#resendotp").prop("disabled",false);
                }
              }, 1000);
            } 
            else {
              console.log(resp.type);
            }
          }
        },
        error: function (XMLHttpRequest, textStatus) {
          $("#verify_otp .verify_error").text("Please Try Again").show();
       }
     };
     jQuery.ajax(params);
    };

    simply.sendOtp = function () {
      {% assign mob_num = checkout.shipping_address.phone %}
      var mob_number = {{ mob_num }};
      mob_number = parseInt(mob_number);
      simply.ajaxSubmit(mob_number);
    };

  simply.showOtpBlock = function () {
     var verify_otp_show = $(codInput).is(":checked");
    if (verify_otp_show) {
      $('#verify_otp').show();
      $('#verify_otp').css('opacity',"1");
      simply.sendOtp();
    }
    else {
      $('#verify_otp').hide();
    }
  };

  simply.codotpClickEvent = function(){
    $(document).on("click", ".edit_checkout .step__footer__continue-btn", function (e) {
      if($(codInput).is(":checked") && window.otpverified == false){
        e.preventDefault();
        var attribute = $(this).attr("data-trekkie-id");
        if(attribute == "complete_order_button"){
          simply.showOtpBlock(); 
        }
      }
    });

    $("#resendotp").click(function (e) {
      $(".verify_error").hide();
      simply.sendOtp();
    });

    $("#verify_otp .close_otp_box").on("click",function(){
      $(this).closest("#verify_otp").hide();
      location.reload();
    });
     $(document).on("click", "#otpverify",function(){
      var otp_value = $(this).closest(".otp_verify").find(".otp_code").val();
      if(otp_value != ''){
        otp_value = parseInt(otp_value);
        {% assign mob_num = checkout.shipping_address.phone %}
        var mob_number = {{ mob_num }};
        mob_number = parseInt(mob_number);

        simply.otpVerifyAjaxSubmit(otp_value,mob_number);
      }
      else{
        $("#verify_otp .verify_error").text("Please Enter OTP..").show();
      }
     });
  };
  $(document).ready(function(){
    simply.codotpClickEvent();
  });
}
</script>
